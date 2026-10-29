# Neat Download Manager 中文套件（Windows）

面向 Windows 的 **Neat Download Manager（NDM）** 一站式使用套件，由三部分组成：

| 组成 | 说明 |
|------|------|
| 🔗 **下载器本体** | 官方 Neat Download Manager 安装包（本项目不包含，需从官网下载） |
| 🈶 **NeatDownloadManager-zh** | 桌面端界面 **简体中文汉化补丁**，支持一键汉化 / 一键还原英文 |
| 🧩 **NeatDownloadManager-Extension-Edge / -Chrome / -Firefox** | 浏览器扩展三件套，接管下载链接，并支持**按网站屏蔽**下载弹窗 |

> Neat Download Manager 是一款免费的多线程下载管理器，支持断点续传、浏览器集成、代理、限速等功能。官方仅提供英文界面，本仓库为其 Windows 版补充中文界面与更顺手的浏览器集成。

---

## 来源与上游

| 模块 | 上游来源 | 说明 |
|------|----------|------|
| 汉化补丁 | [qiqi258/NeatDownloadManager-zh](https://github.com/qiqi258/NeatDownloadManager-zh) | `NeatDownloadManager-zh/` 全部内容来自该项目 |
| Edge / Chrome 扩展 | [yizhimuzhuozi/NeatDownloadManager-Extension](https://github.com/yizhimuzhuozi/NeatDownloadManager-Extension) | 在官方扩展基础上加入“按网站屏蔽下载”与 Ctrl 绕过 |
| Firefox 扩展 | 由上述代码适配（MV2） | 另有更完整的跨浏览器分支：[iiHawe/NeatDownloadManager-Extension](https://github.com/iiHawe/NeatDownloadManager-Extension) |
| 下载器本体 | [neatdownloadmanager.com](https://neatdownloadmanager.com) | 官方免费软件，本仓库不包含 |

---

## 目录结构

```
NDM/
├── NeatDownloadManager-zh/                  # NDM 界面简体中文汉化补丁
│   ├── 中文汉化安装器.bat                    # 一键汉化（自动请求管理员权限）
│   ├── 恢复英文原版.bat                      # 一键还原英文
│   ├── translate_ndm_win.py                 # 核心汉化脚本（翻译表 + 替换逻辑）
│   ├── restore_ndm_win.py                   # 从备份还原英文原版
│   ├── analyze_ndm.py                       # 开发用：提取 exe 内英文字符串
│   ├── verify_layout.py                     # 开发用：验证字符串字节布局
│   ├── LICENSE                              # MIT
│   └── README.md                            # 汉化详细说明
├── NeatDownloadManager-Extension-Edge/      # Edge 扩展（拖入即可）
│   ├── manifest.json                        # MV3 + service_worker
│   ├── bg.js / ct.js                        # 后台服务 + 内容脚本
│   ├── options.html / options.js            # 屏蔽网站管理页
│   ├── img/                                 # 图标资源
│   └── README.md                            # 功能说明与更新日志
├── NeatDownloadManager-Extension-Chrome/    # Chrome 扩展（代码同上，拖入即可）
└── NeatDownloadManager-Extension-Firefox/   # Firefox 扩展（Manifest V2，两种装法）
    └── neatdownloadmanager_extension-1.9.9.xpi   # 官方签名包，拖入即永久安装
```

> 三份扩展的业务代码完全相同。Firefox 版采用 **Manifest V2**（与官方 Firefox 扩展一致：持久后台页 + `browser_action`），其余仅改 `manifest.json`。**按自己用的浏览器拖对应的一份，不要混用。**

---

## 快速上手（三步）

### 第一步：下载并安装 NDM 本体

1. 打开官网下载页：<https://neatdownloadmanager.com/index.php/en/>
2. 下载 Windows 版安装包并安装
   - 默认安装路径：`C:\Program Files (x86)\Neat Download Manager\`
3. 启动一次 NDM，确认能正常打开

> 汉化补丁默认按上面的路径查找 `NeatDM.exe`。如果你的安装目录不同，请修改 `translate_ndm_win.py` 与 `restore_ndm_win.py` 顶部的 `EXE_PATH`。

### 第二步：安装浏览器扩展

**Edge / Chrome（拖入即可）**

1. 下载本仓库，解压后得到三个扩展目录，按浏览器选一个：
   - Edge → `NeatDownloadManager-Extension-Edge`
   - Chrome → `NeatDownloadManager-Extension-Chrome`
2. 浏览器地址栏打开扩展管理页：
   - Edge：`edge://extensions/`
   - Chrome：`chrome://extensions/`
3. 打开 **开发者模式** 开关
4. 把对应文件夹 **直接拖入** 页面，松开即安装
5. 在工具栏把扩展 **固定**，方便随时查看状态

> ⚠️ **这个文件夹不要删除、不要移动**，否则扩展会失效，建议放到一个固定位置。
>
> 若提示"无法加载扩展程序"，请确认拖入的是 **文件夹本身**（里面直接有 `manifest.json`），而不是压缩包或它的上一级目录。

**Firefox（两种选一个，别同时启用）**

Firefox 不支持拖文件夹加载扩展，只能选下面一种：

- **要永久生效**：把 `NeatDownloadManager-Extension-Firefox/neatdownloadmanager_extension-1.9.9.xpi` **直接拖进 Firefox 窗口** → 点「添加」。这是官方签名包，重启后依然在，但没有屏蔽网站功能。
- **要屏蔽功能**：地址栏打开 `about:debugging#/runtime/this-firefox` → 点 **「临时载入附加组件…」** → 选中 `NeatDownloadManager-Extension-Firefox/manifest.json`。未签名，**重启浏览器后会消失**，需重新载入。

详见 [Firefox 说明](NeatDownloadManager-Extension-Firefox/README.md)。

### 第三步：汉化界面（可选）

1. **关闭** 正在运行的 Neat Download Manager
2. 双击 `NeatDownloadManager-zh/中文汉化安装器.bat`
3. 弹出权限确认时点 **是**
4. 脚本自动完成：检查环境 → 关闭 NDM → 备份原 exe → 汉化
5. 看到"汉化完成"后启动 NDM，界面即为中文

也可以手动运行（需管理员权限的命令行）：

```cmd
cd /d "NeatDownloadManager-zh"
python translate_ndm_win.py
```

**还原英文原版**：双击 `NeatDownloadManager-zh/恢复英文原版.bat`，或手动运行 `python restore_ndm_win.py`。

> 汉化只修改界面显示文字，不改动任何功能逻辑；替换前后 exe 文件大小保持不变，并且会先自动备份。

---

## 浏览器扩展用法

| 操作 | 效果 |
|------|------|
| 网页上正常点击下载链接 | 链接被发送到 NDM 下载 |
| 目标网站 **右键 → `Download`** | 强制用 NDM 下载当前页面的链接 |
| 目标网站 **右键 → `Block downloads`** | 屏蔽该网站，之后该站的下载全部交给浏览器原生下载，不再弹出 NDM 下载框 |
| 目标网站 **右键 → `Unblock downloads`** | 解除屏蔽 |
| 按住 **Ctrl** 点击下载链接 | 临时绕过 NDM，用浏览器原生下载 |

- 屏蔽后若页面无反应，**刷新一次页面** 即可生效
- 屏蔽列表可在扩展的"选项"页（Blocked Websites）里查看和删除
- 适合屏蔽那些会弹出大量小文件下载框的站点

更详细的说明见：[Edge](NeatDownloadManager-Extension-Edge/README.md) ・ [Chrome](NeatDownloadManager-Extension-Chrome/README.md) ・ [Firefox](NeatDownloadManager-Extension-Firefox/README.md)。

---

## 常见问题

**Q：汉化脚本提示"没有写入权限"？**
A：修改 `Program Files` 下的文件需要管理员权限。双击 `中文汉化安装器.bat` 会自动请求提权；或右键"以管理员身份运行"打开命令行后再执行 Python 脚本。

**Q：汉化后 NDM 打不开 / 闪退？**
A：运行 `恢复英文原版.bat` 还原，然后重新汉化。若仍异常，请附上汉化脚本的完整输出反馈。

**Q：部分界面还是英文？**
A：部分文字是程序运行时动态拼接的，不在静态字符串中，或中文比原文长而按安全策略跳过。可运行 `analyze_ndm.py` 导出字符串清单，在 `translate_ndm_win.py` 的 `TRANSLATIONS` 中补充或缩短译文。

**Q：NDM 升级后中文没了？**
A：覆盖安装会替换 exe。升级后重新运行一次汉化脚本即可。

**Q：杀毒软件报警？**
A：补丁直接修改 exe 二进制，可能被启发式引擎误报，属正常现象，可添加信任或改用还原脚本。

**Q：扩展装了但下载不走 NDM？**
A：① 确认 NDM 正在运行；② 确认站点没有被 `Block downloads` 屏蔽；③ 在扩展选项页检查屏蔽列表；④ 重新加载扩展（`edge://extensions/` / `chrome://extensions/` 点刷新）；⑤ Firefox 上确认没同时启用两份扩展。

**Q：支持 macOS 吗？**
A：本仓库只针对 **Windows 版**。macOS 版汉化请见下方参考项目。

---

## 技术原理（简述）

- **汉化**：NDM 的界面文字以 UTF-16LE 硬编码在 `NeatDM.exe` 中。脚本把英文按"字符串 + 空终止符"精确匹配定位，再用中文替换；中文较短时用 `\x00` 填充，中文过长则跳过，保证文件长度不变、不破坏结构。详见 [NeatDownloadManager-zh/README.md](NeatDownloadManager-zh/README.md)。
- **扩展**：`bg.js` 监听 `webRequest` / `webNavigation` 拦截下载并通过 WebSocket（`ws://127.0.0.1:10007`）转发给 NDM，`ct.js` 注入页面提供下载面板与屏蔽逻辑，屏蔽列表通过 `chrome.storage` 持久化。Edge/Chrome 为 Manifest V3（`service_worker` 后台），Firefox 为 Manifest V2（持久后台页 + `chrome.browserAction`，代码逻辑一致）。

---

## 环境要求

- Windows 10 / 11
- Neat Download Manager 1.4.24（Windows 版）
- Python 3.6+（仅汉化需要，安装时勾选 **Add Python to PATH**）
- Edge / Chrome 等 Chromium 内核浏览器（扩展）

---

## 声明

> ⚠️ 本项目仅提供汉化补丁与浏览器扩展，**不包含** Neat Download Manager 本体。
> Neat Download Manager 的版权归原作者 **Javad Motallebi** 所有。
>
> 汉化脚本、分析工具、安装脚本及文档借助 AI 工具生成并经人工测试验证，请自行评估风险后使用。

## 致谢

- [Neat Download Manager](https://neatdownloadmanager.com) —— Javad Motallebi 开发的优秀免费下载管理器
- [qiqi258/NeatDownloadManager-zh](https://github.com/qiqi258/NeatDownloadManager-zh) —— 汉化补丁来源
- [hardwin2023/NeatDownloadManager-zh](https://github.com/hardwin2023/NeatDownloadManager-zh) —— macOS 版汉化，本项目参考其思路
- [yizhimuzhuozi/NeatDownloadManager-Extension](https://github.com/yizhimuzhuozi/NeatDownloadManager-Extension) —— Edge/Chrome 扩展来源，“屏蔽网站”功能作者
- [iiHawe/NeatDownloadManager-Extension](https://github.com/iiHawe/NeatDownloadManager-Extension) —— Firefox 方向的跨浏览器分支，功能更全

## 许可证

本项目代码采用 [MIT](NeatDownloadManager-zh/LICENSE) 许可证发布，可自由使用、修改和分发。
