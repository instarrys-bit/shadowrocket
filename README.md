# Shadowrocket 分流配置与 YouTube 本地脚本

本仓库包含一份 Shadowrocket 配置，以及用于 YouTube 请求/响应处理的 JavaScript 脚本。当前以**可审查、可维护**为目标；不承诺适用于所有 iOS 版本、Shadowrocket 版本或 YouTube 接口状态。

## 文件说明

| 文件 | 用途 |
|---|---|
| `best_local.conf` | 完整 Shadowrocket 配置，包含 DNS、分流规则、MITM 与脚本规则。不是单纯的 YouTube 模块。 |
| `youtube_request_local.js` | 基于 Maasea/sgmodule 相关脚本修改的请求脚本；移除了第三方 Worker 重定向，相关流程改为本地回退处理。 |
| `youtube_response.js` | YouTube 响应处理脚本。 |

## 重要：仓库可见性与脚本路径

检查时 GitHub API 将此仓库报告为 **Private（私有）**。如果 Shadowrocket 需要通过未经身份验证的 HTTPS/Raw URL 下载脚本，私有仓库通常无法直接提供给它；不要把带令牌的 URL 或访问令牌写进配置文件。若要远程托管脚本，应先决定是否公开脚本仓库，或改用你信任且支持访问控制的托管方式。公开仓库会让其中的配置与代码对所有人可见，发布前先检查是否包含订阅链接、节点凭据、个人信息或其他秘密。


`best_local.conf` 中的 `script-path` 是示例/待配置路径，当前写法为：

- `youtube.response.js`
- `youtube.request.local.js`

而仓库中的实际文件名是：

- `youtube_response.js`
- `youtube_request_local.js`

**导入前必须让配置中的脚本路径与实际可访问路径一致。** Shadowrocket 不会因为脚本存在于 GitHub 仓库，就自动把它们加载到本地；请根据你实际使用的托管方式/本地脚本导入方式配置路径。不要直接假定私有仓库的 Raw URL 可供 Shadowrocket 访问。

## 安全与测试状态

- `best_local.conf` 是完整配置；更换它可能改变 DNS、国内外分流、MITM 与其他网络行为。
- MITM 仅应在你理解其影响的前提下启用；按需安装并信任自己生成的证书。不要安装来源不明的根证书。
- 任何脚本规则都可能因 YouTube 接口变化而失效，也可能增加请求处理开销。
- 本仓库当前没有提供真实设备的完整回归测试证明。修改后请逐项验证：常用国内应用直连、海外服务可用、YouTube 首页/搜索/播放/字幕、DNS 与 IP 出口行为。
- 出现问题时先停用新增脚本规则或恢复备份，不要为了排错一次性覆盖整个配置。

## 建议的使用流程

1. 先备份当前正在使用的 Shadowrocket 配置。
2. 在副本上检查 DNS、分流、MITM 和脚本规则。
3. 按上面的文件名说明配置正确的脚本路径。
4. 先验证基础联网与国内应用，再验证 YouTube 功能。
5. 确认稳定后再替换日常使用的配置。

## 上游来源与许可

YouTube 脚本修改参考了 [Maasea/sgmodule](https://github.com/Maasea/sgmodule)。上游项目采用 Apache-2.0 许可。分发或继续修改相关代码时，请保留适用的版权、许可与修改说明，并核对脚本文件内的原始声明。此仓库的修改不代表上游作者背书。

## 免责声明

此项目仅用于个人配置研究与互操作性测试。网络规则、域名、接口和第三方服务行为会变化；请自行评估隐私、安全、服务条款及当地法律风险。
