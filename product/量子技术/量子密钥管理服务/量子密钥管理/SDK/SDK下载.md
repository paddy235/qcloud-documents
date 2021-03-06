### SDK 下载
腾讯云 QMS 目前支持 java、python、php 及 C++ SDK, 后续会支持更多语言。也欢迎广大开发者根据 API 说明开发更多语言的 SDK 版本。

github 地址如下：
- [Java SDK](https://github.com/tencentyun/kms-java-sdk)
- [Python SDK](https://github.com/tencentyun/kms-python-sdk)
- [PHP SDK](https://github.com/tencentyun/kms-php-sdk)
- [C++ SDK](https://github.com/tencentyun/kms-cpp-sdk)

下载地址如下：
- [Java SDK](https://cloud.tencent.com/document/product/847/16557)
- [Python SDK](https://cloud.tencent.com/document/product/847/16559)
- [PHP SDK](https://cloud.tencent.com/document/product/847/16558)
- [C++ SDK](https://cloud.tencent.com/document/product/847/16556)


### SDK 使用注意事项

使用 SDK 前至少要获取 [secret id](https://console.cloud.tencent.com/capi)， [secret key](https://console.cloud.tencent.com/capi)，endpoint（即请求发到哪个地域，走内网还是外网）。

endpoint 说明：	
- 内网 endpoint：`https://kms-region.api.tencentyun.com`
- 公网 endpoint：`https://kms-region.api.qcloud.com`

如果业务进程也部署在腾讯云的 CVM 子机上，强烈建议使用同地域的内网 endpoint。例如在腾讯云北京地域的 CVM 子机则建议您使用 `https://kms-bj.api.tencentyun.com`。

原因如下：
1. 同地域内网时延更低；
2. 目前 QMS 对于公网下行流量是要收取流量费用的，用内网可以节省这部分的费用。
region 需用具体地域替换：gz（广州），sh（上海），bj（北京）。公共参数中的 region 值要与域名的 region 值保持一致，如果出现不一致的情况，以域名的 region 值为准，将请求发往域名 region 所指定的地域。
