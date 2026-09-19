# 自定义TTS

## 请求处理

### 表达式
| 参数              | 名称   | 说明           |
|:----------------|:-----|:-------------|
| `@{voiceType}`  | 语音标志 | 角色的唯一标识      |
| `@{text}`       | 文本内容 | 需要转换为语音的文字内容 |
| `@{voiceStyle}` | 情感标识 | 角色的情感风格标识 |

### 请求示例

#### GET/POST请求
```javascript
// 请求地址
let url = 'https://www.baidu.com'; 

// 请求参数
let params = {
	type: '@{voiceType}',
	text: '@{text}',
	style: '@{voiceStyle}',
};

// 请求header
let header = {
	token: 'xxx'	
};

// POST请求，具体参考文档 native-to-javascript
let response = await app.post({
	url,
	params,
	header
});

// 如果responseBody是一个json字符串
let json = app.stringToJson(response.responseBody);

// 获取到语音文件的地址
let voiceUrl = json.url;

return {
	url: voiceUrl, // 语音请求地址
	method: 'GET', // 请求方式
	params: {}, // 携带的参数
	header: {}, // 携带的header
	cookie: {} // 携带的cookie
};
```

#### WebSocket

WebSocket和post的最大区别在于WebSocket的返回值是字符串 socket

```javascript
const ws = app.socket('wss://xxxxx');

// socket连接
ws.open(() => {
    console.log('连接socket成功');
    let message = '{"type": "@{voiceType}", "text": "@{text}"}'
    // 发送消息
    ws.send(message);
});

// 获取text文本信息
ws.text((text) => {
    console.log('返回的text数据:', text);
    const json = JSON.parse(text);
    if (json.event === 'TaskFinished') {
        // 告诉app，消息已经接收完毕
        ws.finished();
        
        // 关闭连接
        ws.close();
    }
});

// 获取二进制消息信息
ws.binary((data) => {
    console.log('返回的二进制数据:', data.length);
    // 将获取到的数据处理后重新推送给app，如果不做处理，直接push data数据即可
    ws.push(data)
});

// 必定返回值，告诉app这是一个websocket
return 'socket';
```

#### WebSocket 规则协议表

| 项目 | 协议 | 说明 |
| --- | --- | --- |
| 返回值 | 必须 `return 'socket'` | 告诉 App 这是 WebSocket 规则，不要当作普通 POST。 |
| 接收文本 | `ws.open((text) => { ... })` | 文本消息回调；字符串可能是 JSON，需要自行解析。 |
| 接收二进制 | `ws.binary((data) => { ... })` | 二进制回调；典型用于直接推送音频。 |
| 推送音频 | `ws.push(data)` | 把处理后的音频块推回 App。 |
| 正常结束 | `ws.finished()` | 表示本轮文本已经播完；调用后再决定是否 `ws.close()`。 |
| 中断 / 异常 | `ws.close()` | 关闭连接，不表示正常播完。 |
| 失败回执 | 不要把 `finished()` 当作“关闭连接” | `finished()` 仅说明服务端正常返回完毕，连接仍需 `close()`。 |

## 加密音频（CENC）

部分 TTS 源返回的是采用 CENC `cenc-aes-ctr`（ISO/IEC 23001-7）加密的 MP4，直接下载得到的是无法播放的加密文件。App 会完整下载音频，在后台解密并完整解码验证，通过后才写入正式缓存并交给播放器。验证期间会使用临时文件，结束后清理。规则侧只需额外声明 `decrypt` 字段，普通规则无需修改；加密源首次播放会增加解密与验证等待时间。

### decrypt 字段

| 参数 | 名称 | 类型 | 必填 | 说明 |
|:---|:---|:---|:---|:---|
| `type` | 解密方案 | String | 是 | 目前仅支持 `cenc-aes-ctr` |
| `key` | 解密密钥 | String | 是 | 32 位十六进制字符（16 字节） |
| `kid` | 密钥标识 | String | 否 | 32 位十六进制字符；填写后 App 会与文件内标识比对，不一致直接失败，便于提前发现密钥与内容不匹配 |

约束：

- 未声明 `decrypt` 的规则行为完全不变，旧规则无需修改；
- 解密产物固定按 MP4 处理，**不依赖**地址后缀；下载入口仍校验响应 `Content-Type` 和 MP4 文件头；加密音频常见返回 `video/mp4` 而非 `audio/*`，未声明解密时会被判为「文件格式出错」；
- 源本身未加密时**不要**写该字段，否则会尝试解密普通音频而失败；
- 不要在规则日志、截图或公开示例中输出密钥及包含密钥的接口响应。
- `decrypt` 仅允许对象或省略/null；空对象、数组、字符串、数字等非法值会被拒绝。非空 `kid` 必须为 32 位十六进制字符。
- CENC CTR 不提供完整性认证，KID 匹配不代表密钥正确。App 会在缓存写入前验证音频能否完整解码，拒绝解码失败或空音轨；但“可解码”仍不等于密钥认证，也不能保证音频语义正确。

### 请求示例

```javascript
// 本例只演示按条目 ID 下载；开放参数需先声明，不是文本转语音映射。
const itemId = config.openParams.itemId;
if (!itemId) { throw new Error("缺少 itemId 开放参数"); }
// 请求音频信息接口，取得音频地址与解密密钥
const res = await app.get({ url: config.host + '/audio?item_ids=' + itemId, header: {} });
const json = app.stringToJson(res.responseBody);
const video = json.data.videos[0];
const info = video.encrypt_info || {};

const result = {
	url: video.main_url,
	method: 'GET',
	header: {},
	params: {},
	cookie: {}
};

// 仅在接口明确声明加密且下发了密钥时附加解密描述
if (info.encrypt === true && info.decryption_key) {
	result.decrypt = {
		type: info.encryption_method || 'cenc-aes-ctr',
		key: info.decryption_key,
		kid: info.kid
	};
}

return result;
```

示例中的接口地址和响应字段需按实际服务调整。使用前请检查接口状态、响应结构、音频地址及解密字段是否齐全；不要打印或公开包含密钥的接口响应。

### 文本与条目 ID

开放参数需要先在规则中声明，用户填写后可通过 `config.openParams.itemId` 读取。按条目 ID 获取已有音频不等于按文本合成语音；当前 App 按文本分段请求，尚未实现动态章节到条目 ID 的映射。**固定 itemId 仅适合单条音频测试**，直接用于连续阅读会反复返回同一条音频。实际接入需要明确文本或章节到条目 ID 的来源，不能将本示例当作已完成的连续阅读映射。

### 错误提示

| 提示 | 原因与处理 |
| --- | --- |
| `解密密钥非法: 需要 32 位十六进制字符，当前长度 N` | `key` 长度不对，检查接口字段是否取错 |
| `解密密钥非法: 包含非十六进制字符` | `key` 混入非 0-9/a-f 字符 |
| `暂不支持的解密方案: xxx，当前仅支持 cenc-aes-ctr` | `type` 不在支持列表（如 `cbcs`） |
| `decrypt 必须是对象` / `decrypt.type 必须是非空字符串` / `decrypt.key 必须是字符串` | 解密描述的类型或必填字段不合法；不要返回空对象、数组或标量 |
| `密钥标识 kid 必须是 32 位十六进制字符` | 非空 `kid` 格式不合法；省略时不进行 KID 一致性检查 |
| `加密音频解密失败: 解密产物异常: 密钥标识不一致，请核对规则与音频是否来自同一条目` | 填写的 `kid` 与文件内标识不一致；检查接口返回的音频和解密描述是否配套 |
| `加密音频解密失败: 加密音频容器无法解析: ...` | 返回内容不是 CENC 加密 MP4，多为地址失效或返回了错误页 |
| `加密音频解密失败: 未知或不受支持的内容保护方案；当前仅支持 cenc` | 容器内 `schm` 声明未知或非 `cenc` 方案，如 `cbcs`、`cens`、`cbc1` |
| `加密音频解密失败: 解密音频无法完整解码，请检查密钥、文件或系统编码支持` | 解密后音频未通过完整解码验证；可能是密钥错误、文件损坏或当前系统不支持其编码参数 |
| `加密音频解密失败: 解密音频验证超时` | 解码验证循环超过 120 秒协作期限，不发布新缓存 |
| `文件格式出错或请求返回错误` | 规则未声明 `decrypt`，但服务端返回的是 `video/mp4` |

### 支持范围

- 支持单轨音频 `cenc-aes-ctr`，原编码限定为 AAC（`mp4a`）或 Opus；必须提供 `sinf/schm` 声明及内联 `senc`。
- 渐进式 MP4 支持 `stsz/stsc/stco` 或 `co64` 样本定位；fMP4 必须带初始化段，使用 `default-base-is-moof`，每个 `moof` 后紧跟对应 `mdat`。
- 支持 8/16 字节 IV 与有界子样本划分；非空子样本表必须完整覆盖样本。`seig` 分组仅允许与 `tenc` 默认参数完全等价的声明。
- 明确拒绝多轨、多采样描述、密钥轮换、外部数据引用、缺少 `schm`、非 `cenc` 方案，以及只有 `saiz/saio` 而没有 `senc` 的布局。
- 容器解密阶段限制：文件不超过 128 MiB、样本不超过 250,000 个、子样本不超过 1,000,000 个，box 树深度不超过 32。加密下载同时检查响应声明长度和已接收字节，超过 128 MiB 时取消传输；未知长度响应同样受累计字节限制。
- 音频完整下载后才进行解密与验证，不支持边下载边播放。取消或验证失败时不会保存新的可播放缓存，也不会自动删除已有缓存。
- 完整解码会增加首次播放等待时间；验证循环设有 120 秒协作时间上限，系统解码调用本身不能被此期限硬中断。
- 音频能否播放还取决于系统版本及具体编码参数。AAC/Opus 支持范围不代表所有参数组合均可播放，使用前请在目标设备上测试。

## 角色配置

### 参数

| 参数        | 名称   | 类型     | 默认值 | 说明           |
|:----------|:-----|:-------|:----|:-------------|
| voiceStyle | 情感标识 | String | -   | 角色的情感风格标识  |
| voiceType | 语音标识 | String | -   | 角色的唯一标识      |
| sex       | 性别   | Number | 0   | 性别 0未知，1男，2女 |
| name      | 名称   | String | -   | 显示的名称        |

### 示例
```json
[
    {
        "voiceStyle" : "gentle", // 情感风格标识
        "voiceType" : "zh-zhangsan", // 语音标识
        "sex" : 1, // 性别 0未知，1男，2女
        "name" : "张三" // 名字
    },
    {
	    "voiceStyle" : "sad", // 情感风格标识
        "voiceType" : "zh-erya", // 语音标识
        "sex" : 2, // 性别 0未知，1男，2女
        "name" : "二丫" // 名字
    }
]
```

## 本地部署TTS

在没有其它TTS可用的情况下，可以尝试在自己电脑上部署一个TTS

文档采用Docker部署方案，我们从Docker安装开始教学

### 安装Docker

1、下载Docker桌面版，根据不同的电脑系统下载对应的软件，以下是官网地址：

```
https://www.docker.com/products/docker-desktop/
```

2、安装完成后打开软件Docker

3、在您任意电脑位置新建文件夹，命名为 `docker`，即：

- Windows电脑打开 `D` 盘，新建文件夹，命名为 `docker`
- MacOS 可以打开访达 `文稿` 在该目录下新建文件夹，命名为 `docker`
- Linux 用户就不说了，大家都懂

4、在 `docker` 目录下新建一个文件，命名为 `docker-compose.yml`，文件后缀一定是 `.yml`，在该文件中写入以下内容，保存并关闭：

```yaml linenums="1"
services:
  ifreetimeTTS:
    container_name: ifreetimeTTS
    image: yunfinibol/ms-ra-forwarder-for-ifreetime:latest
    restart: unless-stopped
    ports:
      - "3000:3000"
    # 如果可以保持自己的ip或者完整域名不公开的话，可以不用设置环境变量
    environment: []
    #需要的话把上边一行注释，下面两行取消注释
    #environment:
    #  - TOKEN=自定义TOKEN
```

### 安装TTS

打开终端软件使用命令安装TTS

Windows用户打开 `cmd` 软件，mac用户打开 `终端` 软件

cd至新建的 `docker` 目录，运行以下命令，正常情况下可以安装成功：

```
# windows
cd D:\docker

# MacOS OR Linux
cd /路径/docker

# 运行安装命令
docker-compose up --build -d
```

在浏览器打开以下链接，测试是否已经安装成功：

```
http://localhost:3000/api/aiyue?voiceName=zh-CN-XiaoxiaoNeural&text=你好
```

### APP配置TTS

1、打开软件，找到 `语音管理` - `语音制作` ，名称，主地址随便填，切换到 `规则配置` 页面，在 `请求处理` 中输入以下代码：

- 电脑和手机必须在同一个局域网内
- 代码中 `你的ip地址` 需要使用你电脑的ipv4地址（具体可以在百度或者google中输入怎么查看电脑ip），然后将获取到的ip地址替换 `你的ip地址`
- 一般局域网的ip地址是一个 `192.168.x.x` 的样式

```javascript linenums="1"

// 请求地址

let encodeData = encodeURIComponent('@{text}')

let url = `https://你的ip地址:3000/api/aiyue?voiceName=@{voiceType}&text=${encodeData}`

return {
  url,
  method: 'GET'
}

```

2、角色配置，点击添加以下内容，保存即可，然后就可以测试发音了
```
名称：晓晓
角色标识：zh-CN-XiaoxiaoNeural
角色性别：女
```

### 参考

```
https://github.com/yy4382/ms-ra-forwarder-for-ifreetime

https://yfi.moe/post/ifreetime-mstts-selfhost/
```
