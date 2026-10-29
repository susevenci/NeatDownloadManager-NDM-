## NeatDownloadManager-Extension（Firefox 版）

本目录同时提供 **两套** 东西，按需选一个装，**不要同时启用**（否则会重复拦截下载）：

| | 增强版（本仓库） | 官方原版 |
|---|---|---|
| 文件 | `manifest.json` + `bg.js` + `ct.js` + `options.*` + `img/` | `neatdownloadmanager_extension-1.9.9.xpi` |
| 按网站屏蔽下载 | ✅ | ❌ |
| Ctrl 点击临时绕过 | ✅ | ❌ |
| 签名 | 未签名 | ✅ Mozilla 官方签名 |
| 安装方式 | `about:debugging` **临时载入** | **直接拖入** Firefox 窗口，永久安装 |
| 重启浏览器后 | 会消失，需重新载入 | 保留 |

> 建议：想要屏蔽功能 → 装增强版；只想省事、开机就在 → 装官方 `.xpi`。

功能说明与更新日志见 [../NeatDownloadManager-Extension-Edge/README.md](../NeatDownloadManager-Extension-Edge/README.md)。

### 与 Edge / Chrome 版的差异

业务代码（`bg.js` / `ct.js` / `options.*`）与 Edge/Chrome 版一致，改动只有两处：

| 项 | Edge / Chrome | Firefox（本目录） |
|---|---|---|
| Manifest | V3 | **V2**（Firefox 官方扩展同样是 V2，兼容性最好） |
| 后台 | `background.service_worker`（MV3 服务进程） | `background.scripts` + `persistent: true`（持久后台页，WebSocket 不会休眠断连） |
| 工具栏 API | `chrome.action` | `chrome.browserAction`（`bg.js` 顶部用 `var Action = chrome.action \|\| chrome.browserAction` 统一） |
| 权限声明 | `permissions` + `host_permissions` 分开 | 全部合并进 `permissions`（加载时一次性授予） |
| 扩展 ID | 浏览器派生 | `browser_specific_settings.gecko.id = neatdm-zh@extensions.local` |

---

## 装法一：增强版（临时载入）

1. 地址栏打开 `about:debugging#/runtime/this-firefox`
2. 点 **「临时载入附加组件…」**
3. 进入本目录，选中 `manifest.json`，打开
4. 扩展立即生效，已打开的标签页按 **F5** 刷新一次

> ⚠️ 临时载入的扩展 **重启 Firefox 后消失**，需要重新走一遍。另外 `.xpi` 也在同一目录，选文件时注意别把 `.xpi` 当成 `manifest.json` 选错。

---

## 装法二：官方原版（拖入即装，永久生效）

1. 找到本目录下的 `neatdownloadmanager_extension-1.9.9.xpi`
2. **直接拖进 Firefox 窗口**（或 `Ctrl+O` 打开该文件）
3. 弹出"要添加该扩展吗？"提示时点 **添加**

这个包带 Mozilla 签名，正式版 Firefox 可以直接永久安装，重启后依然在。代价是**没有**本仓库新增的屏蔽网站 / Ctrl 绕过功能。

> 如果想在增强版基础上做一次正式安装，需要自行签名：
> ```powershell
> Compress-Archive -Path .\NeatDownloadManager-Extension-Firefox\manifest.json,.\NeatDownloadManager-Extension-Firefox\bg.js,.\NeatDownloadManager-Extension-Firefox\ct.js,.\NeatDownloadManager-Extension-Firefox\options.html,.\NeatDownloadManager-Extension-Firefox\options.js,.\NeatDownloadManager-Extension-Firefox\img -DestinationPath .\neatdm-zh.xpi -Force
> ```
> 但未签名包在正式版仍会被拒绝，需要 `web-ext sign`（AMO 账号）或改用 Firefox Developer Edition 并把 `xpinstall.signatures.required` 设为 `false`。

---

## 使用

| 操作 | 效果 |
|------|------|
| 点击页面上的下载链接 | 链接被发送到 Neat Download Manager |
| 页面上右键 → `Download` | 强制用 NDM 下载该链接 |
| 页面上右键 → `Block downloads` | 屏蔽当前网站（**仅增强版**） |
| 页面上右键 → `Unblock downloads` | 解除屏蔽，刷新页面生效（**仅增强版**） |
| 按住 **Ctrl** 点击链接 | 临时绕过 NDM（**仅增强版**） |
| 点击工具栏图标 | 开 / 关"下载嗅探"（关闭后不再弹下载面板） |

屏蔽列表可在扩展的「选项」页里手动增删。工具栏没有图标时：`about:addons` → 本扩展 → 齿轮 → 「固定到工具栏」。

---

## 常见问题

**Q：重启 Firefox 后扩展不见了？**
A：你装的是增强版（临时载入），本来就只在当前会话有效，重新载入一次；想永久生效就用官方 `.xpi`。

**Q：装了但下载不走 NDM？**
① 确认 Neat Download Manager 正在运行；② 确认站点没被 `Block downloads` 屏蔽；③ 确认没有同时启用两份扩展；④ 在 `about:debugging` 里点「重新载入」重载增强版。

**Q：拖 `.xpi` 进 Firefox 提示"无法安装，未经验证"？**
A：那说明拖入的不是本目录这个官方签名包。官方包是已经签过名的，正常不会有这个提示；被拒的是你自己压缩出来的未签名包。

**Q：改了代码怎么生效？**
A：`about:debugging#/runtime/this-firefox` 里找到本扩展，点 **「重新载入」**，然后刷新网页。
