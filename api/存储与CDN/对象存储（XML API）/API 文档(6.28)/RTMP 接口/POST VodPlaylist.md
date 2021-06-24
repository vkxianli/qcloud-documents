## 功能描述

POST VodPlaylist 接口用于为指定通道（Live Channel）生成一个可供点播例用的播放列表。COS 会查询请求参数中时间范围内由该 LiveChannel 推流生成的 ts 文件，由此生成一个 m3u8播放列表，并上传至该存储桶内。

## 请求

#### 请求示例

```plaintext
POST /<ChannelName>/<PlaylistName>?vod&starttime=StartTime&endtime=EndTime HTTP 1.1
Host: <BucketName-APPID>.cos.<Region>.myqcloud.com
Date: GMT date
Content-Length: Content Size
Content-Md5: Content MD5
Authorization: Auth String

```

> ? Authorization: Auth String （详情请参见 [请求签名](https://cloud.tencent.com/document/product/436/7778) 文档）。


#### 请求参数

| 名称         | 描述                                                         | 类型   | 是否必选 |
| ------------ | ------------------------------------------------------------ | ------ | :------- |
| ChannelName  | 指定 LiveChannel 名称，该 LiveChannel 必须存在                   | String | 是       |
| PlaylistName | 指定生成的用于点播的 m3u8文件的名称，必须以`.m3u8`结尾，<br/>长度范围为[6, 128]，默认值：playlist.m3u8。<br/>说明：生成的播放列表名（对象名）为`ChannelName/PlaylistName` | String | 是       |
| StartTime    | 指定查询 ts 文件的起始时间，格式为 Unix timestamp，单位：秒  | String | 是       |
| EndTime      | 指定查询 ts 文件的终止时间，单位：秒。<br/>注意：EndTime 必须大于 StartTime，且时间跨度不能大于1天 | String | 是       |



#### 请求头

此接口仅使用公共请求头部，详情请参见 [公共请求头部](https://cloud.tencent.com/document/product/436/7728) 文档。

#### 请求体

该请求的请求体为空。

## 响应

#### 响应头

此接口仅返回公共响应头部，详情请参见 [公共响应头部](https://cloud.tencent.com/document/product/436/7729) 文档。

#### 响应体

此接口响应体为空。

#### 错误码

此接口遵循统一的错误响应和错误码，详情请参见 [错误码](https://cloud.tencent.com/document/product/436/7730) 文档。



## 实际案例

#### 请求

```plaintext
POST /test-channel-0/playlist123.m3u8?vod\&starttime=1606523814\&endtime=1606552614 HTTP 1.1
Host: examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com
Date: GMT date
Content-Length:Content Size
Content-Md5:Content MD5
Authorization: Auth String

```

#### 响应

```plaintext
HTTP/1.1 200 OK
Content-Type: application/xml
Content-Length: 0
Connection: keep-alive
Date: Wed, 23 Aug 2020 08:14:53 GMT
Server: tencent-cos
x-cos-request-id: NTk5ZDM5N2RfMjNiMjM1MGFfMmRiX2Y0****
```

生成的点播播放列表对象名为：test-channel-0/playlist123.m3u8，其对象内容为：

```plaintext
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-MEDIA-SEQUENCE:0
#EXT-X-TARGETDURATION:3
#EXTINF:3,
1606549744437_1_3000.ts
#EXTINF:3,
1606549747445_1_3000.ts
#EXTINF:3,
1606549750431_1_3000.ts
#EXTINF:3,
1606549753441_1_3000.ts
#EXTINF:3,
1606549756447_1_3000.ts
#EXTINF:3,
1606549759438_1_3000.ts
#EXTINF:3,
1606549762443_1_3000.ts
#EXTINF:0.947,
1606549763773_1_947.ts
#EXTINF:3,
1606551019369_1_3000.ts
#EXTINF:2.307,
1606551021973_1_2307.ts
#EXTINF:1.957,
1606551134831_1_1957.ts
```



