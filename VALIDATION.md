# 1.0.14 社交功能试验版验证

基于本地 1.0.13；2026-11-07。

## 扩展性结论
宿主通过导出函数检测能力，已提供 savePlaylist({id,on}) 和 postComment({trackId,text}) 的适配、界面、登录检查和失败处理。无需改动或重编译 Go 客户端，本次未配置 Go、未运行 Go 测试。

## 实现与验证
新增收藏/取消收藏他人歌单和发表歌曲评论；复用登录会话、CSRF 与 EAPI 请求。校验输入，服务端未明确确认时不报告成功，写入失败不自动重试。
Node 模拟运行时行为测试：25 项通过。覆盖接口参数、评论映射、未登录、无效输入、风控/HTTP错误、缺失评论确认；没有使用真实账号执行社交写操作，未做原生 UI 实测。

## 已知限制
上游接口使用 checkToken v2，本版尚未实现该令牌，也没有验证可靠的免令牌替代路径。因此这是试验性接口接入，不能保证真实账号收藏或发评论成功。官方客户端完成验证后也不保证第三方请求获准。
协议参考：
- https://raw.githubusercontent.com/neteasecloudmusicapienhanced/api-enhanced/master/module/playlist_subscribe.js
- https://raw.githubusercontent.com/neteasecloudmusicapienhanced/api-enhanced/master/module/comment.js

## 安装与人工验证
安装 dist/netease-plugin-1.0.14.zip 并重启客户端，登录后打开他人歌单查看收藏入口、打开歌曲评论查看输入框。由用户自行选择内容执行写入，并在官方客户端核实结果。失败时核实是否已写入，避免重复提交。
原 1.0.13 包保留，可重新安装回退。
