## 功能描述

COS 支持为已存在的存储桶设置对象锁定。PUT Bucket ObjectLockConfiguration 接口用于为存储桶设置对象锁定功能，以满足合规需求。

## 请求

#### 请求示例

```plaintext
PUT /?object-lock HTTP/1.1
Host: <BucketName-APPID>.cos.<Region>.myqcloud.com
Date: GMT Date
Authorization: Auth String 
```

>?Authorization: Auth String（详情请参见 [请求签名](https://cloud.tencent.com/document/product/436/7778) 文档）。

#### 请求头

此接口仅使用公共请求头部，详情请参见 [公共请求头部](https://cloud.tencent.com/document/product/436/7728) 文档。

#### 请求体

```shell
<?xml version="1.0" encoding="UTF-8" ?>
<ObjectLockConfiguration>
       <ObjectLockEnabled>Enabled</ObjectLockEnabled> 
              <Rule> 
                    <DefaultRetention>
                         <Days>30</Days> 
                    </DefaultRetention> 
              </Rule> 
       </ObjectLockConfiguration> 
```

具体数据描述如下：

| 节点名称（关键字）      | 父节点                                        | 描述                                  | 类型       | 是否必选 |
| ----------------------- | --------------------------------------------- | ------------------------------------- | ---------- | -------- |
| ObjectLockConfiguration | 无                                            | 对象锁定配置                          | Container  | 是       |
| ObjectLockEnabled       | ObjectLockConfiguration                       | 是否开启对象锁定                      | String     | 是       |
| Rule                    | ObjectLockConfiguration                       | 对象锁定规则                          | Containers | 是       |
| DefaultRetention        | ObjectLockConfiguration.Rule                  | 对象锁定默认周期                      | Containers | 是       |
| Days                    | ObjectLockConfiguration.Rule.DefaultRetention | 对象锁定默认周期时长（范围为1-36500） | Int        | 是       |

 

## 响应

#### 响应头

此接口仅返回公共响应头部，详情请参见 [公共响应头部](https://cloud.tencent.com/document/product/436/7729) 文档。

#### 响应体

该响应体为空。

#### 错误码

此接口遵循统一的错误响应和错误码，除了以下错误信息，其他错误码请参见 [错误码](https://cloud.tencent.com/document/product/436/7730) 文档。

| HTTP 状态码  | 错误码            | 描述                                                         |
| :----------- | :---------------- | :----------------------------------------------------------- |
| 409 Conflict | InvalidLockedTime | 当 Days 天数小于原有时间，会返回报错：存储桶对象锁定时间不能小于原有时间，该值必须在 1 - 36500 天之间 |







 

 