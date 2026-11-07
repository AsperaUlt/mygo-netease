# 网易云音乐 · MyGo Music 插件（修改版）

适配 [MyGo Music](https://github.com/egoist/mygo-music) 的网易云音乐源插件。本项目基于 [hubandad/mygo-music-netease](https://github.com/hubandad/mygo-music-netease) 修改，保留上游 MIT 许可及版权声明；这是独立维护的修改版，不代表上游作者或网易云官方。

当前本地版本：**1.0.14，社交功能试验版**。本次修改基于工作区中的 1.0.13，未将该本地版本认定为上游已发布标签。

> 收藏他人歌单、发表评论目前仅完成接口接入和模拟测试。网易云接口涉及 checkToken v2，本版尚未实现该令牌，不能保证真实账号写入成功。请勿将本版描述为社交功能稳定可用。

## 功能

- 账号：扫码登录、网页登录、登录状态验证、退出登录。
- 发现与搜索：每日推荐、推荐歌单、排行榜、私人 FM；搜索歌曲、专辑、歌手、歌单与播客。
- 播放与歌词：音质请求、LRC 和逐字歌词；实际播放权限及音质由网易云和账号决定。
- 音乐库：喜欢的音乐、最近播放、我的歌单、专辑收藏、歌手关注。
- 歌单与播客：歌单编辑、播客浏览与订阅。
- 评论：读取歌曲评论、点赞与取消点赞。
- **1.0.14 试验新增**：收藏／取消收藏他人歌单、发表歌曲评论。

新增社交操作复用已有登录会话，校验输入并检查服务端确认信息；错误会反馈给用户，不自动重试写入。宿主已有相应界面及能力检测，无需为这两个接口重新编译 Go 客户端。

## 安装

安装使用无需 Node.js、Go、npm 或额外 API 服务。

1. 从[本修改版 Releases](https://github.com/AsperaUlt/mygo-netease/releases)（发布后）下载 `netease-plugin-1.0.14.zip`。
2. 打开 MyGo Music → 设置 → 插件 → 添加，选择本机 ZIP 安装。
3. 确认网络权限，切换到网易云音乐源；需要账号功能时点击登录并扫码。
4. 更新后完全退出客户端再重新启动。

也可选择源码目录安装，确保 `plugin.json` 与 `index.js` 位于同一层。

扫码无法完成时，在插件设置的“登录方式”填写 `web`，使用网页登录；留空或填写 `qr` 恢复扫码方式。

从宿主 GitHub 安装入口安装时，填写 `AsperaUlt/mygo-netease`。上游链接只用于来源说明，不是本修改版的安装地址。

本版保留插件 ID `netease`，应作为原插件的替换版本使用，不要假定两个版本能够同时安装。更新前建议备份宿主插件数据。

## 验证与限制

- `npm test`：25 项模拟行为测试通过，包含社交请求参数、返回值映射、输入校验及失败不重试。
- 未使用真实账号执行社交写操作，未完成原生界面实测。
- checkToken v2 尚未实现；即使在官方客户端完成验证，也不保证第三方请求获准。
- 写入超时或返回信息不完整时，请先在官方客户端核实结果，避免重复发表评论。
- 本轮未运行 Go 宿主测试；部分此前的最近播放即时刷新改动依赖配套客户端，不保证任意宿主版本具有相同行为。
- 不提供节目红心或远程播放能力；不上传新的听歌打点。

详细记录见 [VALIDATION.md](VALIDATION.md)。建议以 GitHub **Pre-release** 发布本版。

## 权限与数据

插件仅声明 `music.163.com` 和 `interface.music.163.com` 网络权限，不申请进程权限。音频与封面由宿主加载。

登录 Cookie、账号信息及本地播放记录由宿主保存在插件数据目录。不要将这些运行数据上传到 GitHub，也不要在问题反馈中粘贴 Cookie 或完整请求头。插件退出登录会清除本插件的会话，不注销其他网易云客户端。

## 开发

测试需要 Node.js 22 或以上版本。在本仓库根目录运行：

```sh
npm test
```

可选游客只读联网检查（不属于上述25项模拟测试）：

```sh
node --experimental-vm-modules tests/smoke.mjs
```

主要文件：

| 文件 | 用途 |
| --- | --- |
| `index.js` | 插件实现，无外部运行依赖 |
| `plugin.json` | 版本、权限和导航配置 |
| `tests/` | 行为测试及宿主测试样例 |
| `PLUGIN_API.md` | 宿主接口适配说明 |
| `VALIDATION.md` | 验证记录与已知限制 |
| `LICENSE`、`NOTICE` | 许可、版权和第三方来源声明 |

仓库根目录直接放置上述文件，不要把整个开发工作区上传。发布安装 ZIP 时，根目录至少包含 `index.js`、`plugin.json`、`LICENSE` 和 `NOTICE`，建议同时附带 README 与验证说明。

## 来源与许可

- 原插件：[hubandad/mygo-music-netease](https://github.com/hubandad/mygo-music-netease)。
- 宿主：[egoist/mygo-music](https://github.com/egoist/mygo-music)。
- 网易云接口参考：[NeteaseCloudMusicApiEnhanced/api-enhanced](https://github.com/NeteaseCloudMusicApiEnhanced/api-enhanced)，版权及许可见 [NOTICE](NOTICE)。

本修改版继续采用 [MIT License](LICENSE)，保留上游版权声明 `Copyright (c) 2026 mygo-music-netease contributors` 及第三方声明。再分发源码或安装包时，请一并保留这些声明与许可文本。
