## webaudioapi

- [webaudioapi.com](https://webaudioapi.com/book/Web_Audio_API_Boris_Smus_html/toc.html)

- [译文-web audio api 入门](https://www.cnblogs.com/hanshuai/p/13620908.html)

- [Getting Started with Web Audio API](https://www.html5rocks.com/en/tutorials/webaudio/intro/)

- [HTML5 WebAudioAPI简介(一)](https://www.cnblogs.com/tianma3798/p/6033613.html)

- [BiquadFilterNode - 表示一个简单的低阶滤波器](https://www.mifengjc.com/api/BiquadFilterNode.html)

- [js实现pcm数据编码](https://github.com/2fps/demo/blob/master/view/2019/04/js实现pcm数据编码/index.html)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>pcmtowav</title>
</head>
<body>
    <div>
        getUserMedia需要https，使用localhost或127.0.0.1时，可用http。
    </div>
    <button id="start">开始录音</button>
    <button id="end">结束录音</button>
    <button id="play">播放录音</button>
</body>
<script>
var context = null,
    inputData = [],
    size = 0,
    audioInput = null,
    recorder = null,
    dataArray;

document.getElementById('start').addEventListener('click', function() {
    context = new (window.AudioContext || window.webkitAudioContext)();
	// 清空数据
	inputData = [];
    // 录音节点
    recorder = context.createScriptProcessor(4096, 1, 1);

    recorder.onaudioprocess = function(e) {
        // getChannelData返回Float32Array类型的pcm数据
        var data = e.inputBuffer.getChannelData(0);

        inputData.push(new Float32Array(data));
        size += data.length;
    }

    navigator.mediaDevices.getUserMedia({
        audio: true
    }).then((stream) => {
        audioInput = context.createMediaStreamSource(stream);

    }).catch((err) => {
        console.log('error');
    }).then(function() {
        audioInput.connect(recorder);
        recorder.connect(context.destination);
    });
});
document.getElementById('end').addEventListener('click', function() {
    recorder.disconnect();
});
document.getElementById('play').addEventListener('click', function() {
    recorder.disconnect();
    if (0 !== size) {
        // 组合数据
        // var data = combine(inputData, size);
        // console.log(data.buffer);
        // 输出pcm二进制数据
		console.log(encodePCM());
    }
});
// ----------------------
// 以下是增加的内容
var oututSampleBits = 16;  // 输出采样数位

// 数据简单处理
function decompress() {
    // 合并
    var data = new Float32Array(size);
    var offset = 0; // 偏移量计算
    // 将二维数据，转成一维数据
    for (var i = 0; i < inputData.length; i++) {
        data.set(inputData[i], offset);
        offset += inputData[i].length;
    }
    return data;
};
// pcm编码
function encodePCM() {
    let bytes = decompress(),
        sampleBits = oututSampleBits,
        offset = 0,
        dataLength = bytes.length * (sampleBits / 8),
        buffer = new ArrayBuffer(dataLength),
        data = new DataView(buffer);

    // 写入采样数据 
    if (sampleBits === 8) {
        for (var i = 0; i < bytes.length; i++, offset++) {
            // 范围[-1, 1]
            var s = Math.max(-1, Math.min(1, bytes[i]));
            // 8位采样位划分成2^8=256份，它的范围是0-255; 16位的划分的是2^16=65536份，范围是-32768到32767
            // 因为我们收集的数据范围在[-1,1]，那么你想转换成16位的话，只需要对负数*32768,对正数*32767,即可得到范围在[-32768,32767]的数据。
            // 对于8位的话，负数*128，正数*127，然后整体向上平移128(+128)，即可得到[0,255]范围的数据。
            var val = s < 0 ? s * 128 : s * 127;
            val = parseInt(val + 128);
            data.setInt8(offset, val, true);
        }
    } else {
        for (var i = 0; i < bytes.length; i++, offset += 2) {
            var s = Math.max(-1, Math.min(1, bytes[i]));
            // 16位直接乘就行了
            data.setInt16(offset, s < 0 ? s * 0x8000 : s * 0x7FFF, true);
        }
    }

    return data;
}
</script>
</html>
```

https://github.com/pkjy/pcm-player

## PCM音量控制
- [PCM音量控制](https://blog.jianchihu.net/pcm-volume-control.html)

## 音视频博主
- [音视频](https://blog.jianchihu.net/category/avtechnology)

## 使用JDK自带工具keytool生成ssl证书
- [使用JDK自带工具keytool生成ssl证书](https://blog.csdn.net/dwyane__wade/article/details/80350548)

### 一、使用keytool命令生成证书
```shell
keytool -genkey -alias fusionsrv -keypass baonserv -keyalg RSA -keysize 1024 -validity 36500 -keystore fusionsrv.keystore -storepass baonserv

keytool -importkeystore -srckeystore fusionsrv.keystore -destkeystore fusionsrv.keystore -deststoretype pkcs12
```

### 二、为客户端生成证书

```shell
keytool -genkey -alias client -keypass baonserv -keyalg RSA -storetype PKCS12 -keypass baonserv -storepass baonserv -keystore client.p12
```

### 三、让服务器信任客户端证书
必须先把客户端证书导出为一个单独的CER文件，使用如下命令：

```shell
keytool -export -alias client -keystore client.p12 -storetype PKCS12 -keypass baonserv -file client.cer

keytool -import -v -file client.cer -keystore fusionsrv.keystore

keytool -list -v -keystore fusionsrv.keystore
```

### 四、让客户端信任服务器证书
由于是双向SSL认证，客户端也要验证服务器证书，因此，必须把服务器证书添加到浏览器的“受信任的根证书颁发机构”。

由于不能直接将keystore格式的证书库导入，必须先把服务器证书导出为一个单独的CER文件，使用如下命令：

```shell
keytool -keystore fusionsrv.keystore -export -alias fusionsrv -file server.cer
```

双击server.cer文件，按照提示安装证书

将证书填入到“受信任的根证书颁发机构”。具体方法：（我用的谷歌浏览器）：

- 打开谷歌浏览器 --> 设置--> 高级 --> 管理证书 --> 中级证书颁发机构 --> 选择www.seeker.com，点击导出到桌面SEEKER.cer

- 打开谷歌浏览器 --> 设置--> 高级 --> 管理证书 --> 受信任的根证书颁发机构 --> 导入SEEKER.cer

其他浏览器将证书填入到“受信任的根证书颁发机构”：

- 打开浏览器   - 工具  -  internet选项-内容- 证书-把中级证书颁发机构里的www.seeker.com(该名称即时你前面生成证书时填写的名字与姓氏)证书导出来-再把导出来的证书导入  受信任的根颁发机构  就OK了。

### 五、reference

[keytool如何生成自签名证书？](https://blog.csdn.net/u013412772/article/details/103726154)

[[keytool工具生成自签名证书并且通过浏览器导入证书](https://my.oschina.net/u/4365165/blog/4047638)](https://my.os)china.net/u/4365165/blog/4047638

```shell
keytool -genkey -alias fusion -keypass baonserv -keyalg RSA -keysize 1024 -validity 365 -keystore fusionsrv.keystore -storepass baonserv -dname "CN=localhost,OU=shixun,O=shixun,L=beijing,ST=beijing,c=cn"

keytool -genkey -alias client -keypass baonserv -keyalg RSA -keysize 1024 -validity 365 -storetype PKCS12 -keystore client.p12 -storepass baonserv -dname "CN=client,OU=shixun,O=shixun,L=beijing,ST=beijing,c=cn"

keytool -export -alias client -keystore client.p12 -storetype PKCS12 -keypass baonserv -file client.cer -storepass baonserv

keytool -import -v -file client.cer -keystore fusionsrv.keystore -storepass baonserv

keytool -list -v -keystore fusionsrv.keystore -storepass baonserv
```

## webpack-dev-server版本需要把ssl/server.pem转成cer，安装到信任机构
```shell
openssl x509 -inform pem -in server.pem -outform der -out server.cer
```

- [解读 vue-cli 脚手架（一）：npm run dev的背后](https://blog.csdn.net/six_six_six_666/article/details/82633731)

- [vue-cli webpack项目npm run dev启动过程](https://www.cnblogs.com/zeroes/p/vue-run-dev.html)

- [pcm-player](https://github.com/pkjy/pcm-player)

- [获取JavaScript异步函数的返回值](https://www.cnblogs.com/zmc/p/6916164.html)