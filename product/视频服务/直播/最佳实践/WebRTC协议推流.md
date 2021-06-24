TXLivePusher 推流 SDK 主要用于视频云的快直播（超低延迟直播）推流，负责将浏览器采集的音视频画面通过 WebRTC 推送到直播服务器。目前支持摄像头推流、屏幕录制推流和本地媒体文件推流。

## 基础知识

对接前需要了解以下基础知识：

### 推流地址的拼装

使用腾讯云直播服务时，推流地址需要满足腾讯云标准直播推流 URL 的格式 ，如下所示，它由四个部分组成：

![](https://main.qcloudimg.com/raw/44bf2ab0ddae946b440faa4fc2f6d43a.png)

其中鉴权 Key 部分非必需，如果需要防盗链，请开启推流鉴权，具体使用说明请参见  [自主拼装直播 URL](https://cloud.tencent.com/document/product/267/32720) 。

### 浏览器支持

快直播推流基于 WebRTC 实现，依赖于操作系统和浏览器对于 WebRTC 的支持。

除此以外，浏览器采集音视频画面的功能在移动端支持较差，例如移动端浏览器不支持屏幕录制，iOS 14.3及以上版本才支持获取用户摄像头设备。因此推流 SDK 主要适用于桌面端浏览器，目前最新版本的 chrome、Firefox 和 Safari 浏览器都是支持快直播推流的。

移动端建议使用 [移动直播 SDK](https://cloud.tencent.com/document/product/454/56591) 进行推流。

## 对接攻略

### 步骤1：页面准备工作

在需要直播推流的页面（桌面端）中引入初始化脚本。

```html
<script src="https://imgcache.qq.com/open/qcloud/live/webrtc/js/TXLivePusher-1.0.0.min.js" charset="utf-8"></script>;
```

如果在域名限制区域，可以引入以下链接：

```html
<script src="https://cloudcache.tencent-cloud.com/open/qcloud/live/webrtc/js/TXLivePusher-1.0.0.min.js" charset="utf-8"></script>;
```

### 步骤2：在 HTML 中放置容器

在需要展示本地音视频画面的页面位置加入播放器容器，即放一个 div 并命名，例如 id_local_video，本地视频画面都会在容器里渲染。对于容器的大小控制，您可以使用 div 的 css 样式进行控制，示例代码如下：

```html
<div id="id_local_video" style="width:100%;height:500px;display:flex;align-items:center;justify-content:center;"></div>
```

### 步骤3：直播推流
1. **生成推流 SDK 实例：**
通过全局对象 `TXLivePusher` 生成 SDK 实例，后续操作都是通过实例完成。
```javascript
var livePusher = new TXLivePusher();
```
2. **指定本地视频播放器容器：**
指定本地视频播放器容器 div，浏览器采集到的音视频画面会渲染到这个 div 当中。
```javascript
livePusher.setRenderView('id_local_video');
```
3. **设置音视频质量：**
采集音视频流之前，先进行音视频质量设置，如果预设的质量参数不满足需求，可以单独进行自定义设置。
```javascript
// 设置视频质量
livePusher.setVideoQuality('720p');
// 设置音频质量
livePusher.setAudioQuality('standard');
// 自定义设置帧率
livePusher.setProperty('setVideoFPS', 25);
```
4. **开始采集流：**
目前支持采集摄像头设备、麦克风设备、屏幕录制和本地媒体文件的流。当音视频流采集成功时，播放器容器中开始播放本地采集到的音视频画面。
```javascript
// 打开摄像头
livePusher.startCamera();
// 打开麦克风
livePusher.startMicrophone();
```
5. 传入腾讯云快直播推流地址，**开始推流**。
推流地址的格式参考 [腾讯云标准直播 URL](https://cloud.tencent.com/document/product/267/32720) ，只需要将 RTMP 推流地址前面的 `rtmp://` 替换成 `webrtc://` 即可。
```javascript
livePusher.startPush('webrtc://domain/AppName/StreamName?txSecret=xxx&txTime=xxx');
```
6. **停止快直播推流**。
```javascript
livePusher.stopPush();
```
7. **停止采集音视频流**。
```javascript
// 关闭摄像头
livePusher.stopCamera();
// 关闭麦克风
livePusher.stopMicrophone();
```

## 进阶攻略
### 兼容性
SDK 提供静态方法用于检测浏览器对于 WebRTC 的兼容性。
```javascript
TXLivePusher.checkSupport().then(function(data) {  
		// 是否支持WebRTC  
		if (data.isWebRTCSupported) {    
			console.log('WebRTC Support');  
		} else {    
			console.log('WebRTC Not Support');  
		}  
		// 是否支持H264编码  
		if (data.isH264EncodeSupported) {    
			console.log('H264 Encode Support');  
		} else {    
			console.log('H264 Encode Not Support');  
		}
	});
```

### 回调事件通知
SDK 目前提供了回调事件通知，可以通过设置 Observer 来了解 SDK 内部的状态信息和 WebRTC 相关的数据统计。具体内容请参见 [TXLivePusherObserver](https://cloud.tencent.com/document/product/454/56500)。
```javascript
livePusher.setObserver({
// 推流警告信息
onWarning: function(code, msg) {
  console.log(code, msg);
},
// 推流连接状态
onPushStatusUpdate: function(status, msg) {
  console.log(status, msg);
},
// 推流统计数据
onStatisticsUpdate: function(data) {
  console.log('video fps is ' + data.video.framesPerSecond);
}
});
```

### 设备管理

SDK 提供了设备管理实例帮助用户进行获取设备列表、切换设备等操作。
```javascript
var deviceManager = livePusher.getDeviceManager();
// 获取设备列表
deviceManager.getDevicesList().then(function(data) {
data.forEach(function(device) {
		console.log(device.deviceId, device.deviceName);  
	});
});
// 切换摄像头设备
deviceManager.switchCamera('camera_device_id');
```









