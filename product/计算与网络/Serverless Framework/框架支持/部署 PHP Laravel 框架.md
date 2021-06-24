## 操作场景
 腾讯云 [Laravel](https://github.com/laravel/laravel) Serverless Component，支持 Laravel 6.0及以上版本。 



## 操作步骤
### 1. 安装 Serverless CLI
通过 npm 全局安装 [Serverless CLI](https://github.com/serverless/serverless)：
```shell
npm install -g serverless
```

>?以下步骤主要针对命令行部署操作，控制台部署请参考 [控制台部署指南](https://cloud.tencent.com/document/product/1154/50957)。

### 2. 初始化 Laravel 模版项目（可选）
如果您本地并没有 Laravel 项目，可通过以下指令完成 Laravel 项目初始化（本地已有项目可跳过该步骤）：
```
serverless init laravel-starter --name example
cd example
```

### 3. 配置 yml 文件
在项目根目录下，新建 `serverless.yml` 文件。
```sh
touch serverless.yml
```
将下列配置模版粘贴到文件中，实现基本的项目配置。
>?基于您实际部署需要，您可以在 `serverless.yml` 中完成更多配置，yml 文件的配置信息请参考 [ Laravel 组件全量配置](https://github.com/serverless-components/tencent-laravel/blob/master/docs/configure.md)。
```yml
# serverless.yml

component: laravel
name: laravelDemo
app: appDemo
stage: dev

inputs:
  src: ./
  region: ap-guangzhou
  runtime: Php7
  apigatewayConf:
    protocols:
      - http
      - https
    environment: release
```

### 4. 应用部署
通过 `sls deploy` 命令进行部署，并可以添加 --debug 参数查看部署过程中的信息。

```
sls deploy --debug
```
部署完成后，通过访问输出的 API 网关链接，完成对应用的访问。

### 5. 监控运维
部署完成后，您可以通过访问 [Serverless  SSR 控制台](https://console.cloud.tencent.com/ssr)，查看应用的基本信息，监控日志。



<span id="account"></span>
### 账号配置（可选）

当前默认支持 CLI 扫描二维码登录，如您希望配置持久的环境变量/密钥信息，也可以本地创建 `.env` 文件：

```shell
touch .env # 腾讯云的配置信息
```

在 `.env` 文件中配置腾讯云的 SecretId 和 SecretKey 信息并保存：

```text
# .env
TENCENT_SECRET_ID=123
TENCENT_SECRET_KEY=123
```

>?
>- 如果没有腾讯云账号，请先 [注册新账号](https://cloud.tencent.com/register)。
>- 如果已有腾讯云账号，可以在 [API 密钥管理](https://console.cloud.tencent.com/cam/capi) 中获取 SecretId 和 SecretKey。

