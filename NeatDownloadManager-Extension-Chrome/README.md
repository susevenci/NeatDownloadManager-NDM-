## NeatDownloadManager-Extension（Chrome 版）

基于原版 NDM 浏览器扩展，增加了**按网站屏蔽下载**、**Ctrl 点击临时绕过** 两项功能。

本目录与 `NeatDownloadManager-Extension-Edge` 代码完全一致（同为 Chromium / Manifest V3），区别仅是浏览器不同，**选自己用的那个拖入即可，不要混用**。

功能说明与更新日志见 [../NeatDownloadManager-Extension-Edge/README.md](../NeatDownloadManager-Extension-Edge/README.md)。

## Chrome 安装

1. 打开 `chrome://extensions/`
2. 开启右上角的 **开发者模式**
3. 把本目录 `NeatDownloadManager-Extension-Chrome` 直接拖入页面
4. 建议在工具栏固定扩展图标，方便查看"下载嗅探"开关状态

注意：

- 解压后的文件夹**不能删除、不能移动**，否则扩展会失效，请放到固定位置
- 若提示"无法加载扩展程序"，确认拖入的是**文件夹本身**（里面直接有 `manifest.json`），而不是压缩包

## 使用

| 操作 | 效果 |
|------|------|
| 点击页面上的下载链接 | 链接被发送到 Neat Download Manager |
| 页面上右键 → `Download` | 强制用 NDM 下载该链接 |
| 页面上右键 → `Block downloads` | 屏蔽当前网站，之后该站下载全部走浏览器原生下载 |
| 页面上右键 → `Unblock downloads` | 解除屏蔽（需刷新页面生效） |
| 按住 **Ctrl** 点击链接 | 临时绕过 NDM，用浏览器原生下载 |
| 点击工具栏图标 | 开 / 关"下载嗅探"（关闭后不再弹下载面板） |

屏蔽列表可在扩展的「选项」页里手动增删。

## 常见问题

**Q：装了但下载不走 NDM？**
① 确认 Neat Download Manager 正在运行；② 确认站点没被 `Block downloads` 屏蔽；③ 在扩展选项页检查屏蔽列表；④ 到 `chrome://extensions/` 点刷新重载扩展。

**Q：扩展图标上的 "Off" 是什么意思？**
表示"下载嗅探"已关闭（点工具栏图标切换），关闭后网页下载不会再弹到 NDM。
