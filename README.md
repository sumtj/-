# Snipaste

> 一款免费、免安装、无广告的**截图 + 贴图**工具。
> 截图后可以直接把图片「贴」在屏幕最上层，方便对照、参考、拼图。

本目录（`D:\Snipaste`）是一份 **Windows x64 便携版**。

---

## 官方地址

| 用途 | 地址 |
| --- | --- |
| **官网（推荐从这里下载）** | <https://snipaste.com> |
| 中文官网 | <https://zh.snipaste.com> |
| **问题反馈 / 官方 Wiki** | <https://github.com/Snipaste/feedback> |
| 更新日志（Release Notes） | <https://github.com/Snipaste/feedback/releases> |
| 讨论区（Discussions） | <https://github.com/Snipaste/feedback/discussions> |
| 作者 | Liu Lex（[@liulex](https://github.com/liulex)） |

> ℹ️ Snipaste **不是开源软件**。上面那个 GitHub 仓库只用于收集反馈和存放 Wiki，不包含源码。
> 本项目内的程序文件版权归 `snipaste.com` 所有（见下方「版权」）。

---

## 本机版本信息

| 项 | 值 |
| --- | --- |
| 版本 | **2.11.3** |
| 平台 | Windows Desktop (x64) |
| 构建框架 | Qt 6.2.4 |
| 可执行文件 | `Snipaste.exe`（6.68 MB） |
| 发行日期 | 2026-01-18 |
| 版权 | Copyright (C) 2016-2025 snipaste.com |
| 安装方式 | **绿色便携版**（解压即用，未注册到系统「应用和功能」） |
| 首次运行 | 2026-10-04 |
| 目录总大小 | 约 88 MB（含 `.git` 约 30 MB） |

---

## 快速开始

### 启动

双击 `Snipaste.exe`。启动后**没有主窗口**，程序常驻**系统托盘**（右下角）。

### 常用热键（默认）

| 热键 | 功能 |
| --- | --- |
| `F1` | 开始截图 |
| `F3` | 把剪贴板里的图片贴到屏幕上 |
| `Shift` + `F3` | 快速保存（不弹对话框，直接存到快速保存目录） |
| `Ctrl` + `F1` | 检测界面元素（自动识别窗口/控件边界） |
| `Esc` | 取消当前截图 |

> 热键可以在托盘图标右键 →「首选项」→「控制」里修改。

### 截图后能做什么

- 画框、箭头、画笔、马赛克、模糊、文字标注
- 直接复制到剪贴板（`Ctrl` + `C`）
- 保存为文件（`Ctrl` + `S`），支持 PNG / JPG / BMP / GIF / ICO 等
- **贴到屏幕上**（`F3`），可缩放、旋转、设透明度、鼠标穿透

---

## 配置说明

配置文件就在程序目录里：**`config.ini`**（约 113 字节，只保存**被改动过**的项，其余用内置默认值）。

### 本机当前配置

```ini
[General]
first_run=false
read_tips=32

[Snip]
ask_for_confirm_on_esc=false

[Output]
image_quality=100
```

### 关键项解释

| 配置项 | 含义 |
| --- | --- |
| `Output/image_quality` | 输出图片质量。`-1` = 自动（推荐日常使用）；`0` = 体积最小、压缩最狠；`100` = 质量最高、不压缩。**代价是文件会明显变大。** |
| `Snip/ask_for_confirm_on_esc` | 按 `Esc` 时是否二次确认 |
| `General/first_run` | 是否首次运行（`false` = 已运行过） |
| `General/gpu_acceleration` | 是否启用 GPU 加速 |
| `General/high_process_priority` | 是否提高进程优先级 |

> ⚠️ **`image_quality` 只影响「存盘的文件」**，对复制到剪贴板的图片无效 ——
> 剪贴板里的图始终是无损原图（官方明确说明此项目前不提供设置）。

### 本机已做的定制

- **开机自启**：已通过注册表 `HKCU\...\CurrentVersion\Run` 添加：

  ```
  Snipaste = "D:\Snipaste\Snipaste.exe"
  ```

- **输出质量**：`image_quality=100`（最高保真，适合归档截图）

---

## 目录结构

```
D:\Snipaste\
├── Snipaste.exe            # 主程序
├── config.ini              # 配置文件（只存改动过的项）
├── splog.txt               # 运行日志
├── history\                # 贴图历史（.sp0 / .sp1 / .sp2 为私有格式）
├── crashes\                # 崩溃转储（当前为空）
├── lang\                   # 语言包（45 种，.qm 格式）
├── imageformats\           # 图片格式插件（qjpeg / qgif / qsvg / qicns ...）
├── platforms\              # Qt 平台插件（qwindows.dll）
├── styles\                 # Qt 界面样式
├── sound\                  # 音效（snip.wav 截图音、bubble.wav 提示音）
├── tls\                    # 网络 TLS 后端（openssl / schannel）
├── Qt6*.dll                # Qt 6 运行库
├── libssl-3-x64.dll        # OpenSSL 运行库
├── libcrypto-3-x64.dll
├── msvcp140*.dll           # MSVC 运行库
├── vcruntime140*.dll
├── D3Dcompiler_47.dll      # Direct3D 着色器编译器（GPU 加速用）
├── hoedown.dll             # Markdown 渲染（贴图时渲染富文本用）
├── quazip.dll              # 压缩库
└── .git\                   # 本目录已被初始化为 Git 仓库
```

---

## 关于「放大后模糊」——这是常见误解

**截图本身是像素完美的**，模糊来自**贴图窗口放大时的插值**。

### 原因

默认开启「平滑缩放」时，放大图片会用**双线性插值**，像素之间被平均，文字就糊了。

### 解决办法：关闭平滑缩放

关掉后放大变成**硬像素放大**（可能有锯齿，但**清晰锐利**），不糊。

两个入口：

| 范围 | 操作 |
| --- | --- |
| **单张图** | 对着贴图窗口按**鼠标右键** → 取消勾选「平滑缩放」 |
| **全局默认** | 托盘图标右键 →「首选项」→「**粘贴**」标签页 → 取消「平滑缩放」，之后新图默认就是清晰模式 |

> 官方原话：*「取消平滑缩放有时可以改善文字缩放后的显示效果……如果取消这张图片的平滑缩放，可以让文字显示更清晰锐利。」*

### 其他相关设置

| 设置 | 位置 | 说明 |
| --- | --- | --- |
| 输出图片质量 | 首选项 → 输出 | 见上文 `image_quality` |
| 放大镜像素级缩放 | 首选项 → 截图 | 放大镜按像素取色，适合精确选点 |
| HDR 色彩校正 | 首选项 → 高级 | 本机已启用（`Support HDR color correction: true`） |

---

## 开机自启

Snipaste 首选项里那个「开机启动」在本机**没生效**，原因已查明：

> Snipaste 的实现是**创建计划任务**，但它的任务模板里 `<Triggers />` 是**空的** ——
> 没有登录触发器，任务永远不会自己运行。

**因此改用标准注册表自启项**（更可靠，和 OneDrive / 微信 / Steam 同一机制）：

```powershell
# 添加自启
New-ItemProperty -Path 'HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run' `
  -Name 'Snipaste' -Value '"D:\Snipaste\Snipaste.exe"' -PropertyType String -Force

# 取消自启
Remove-ItemProperty -Path 'HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run' -Name 'Snipaste'
```

本机实测：开机后 **27 秒**内自动启动，**静默驻留托盘、不弹窗口**。

> ⚠️ 不要把 `Snipaste.exe` 的属性设为「以管理员身份运行」——
> 官方明确说明这会**破坏开机自启**（提权进程无法从普通权限的用户会话启动）。

---

## 绿色版的维护

因为是便携版，升级/卸载都是直接操作文件：

| 操作 | 做法 |
| --- | --- |
| **升级** | 从[官网](https://snipaste.com)下载新版，解压后**替换 `Snipaste.exe` 和 `Qt6*.dll`**（`config.ini` 保留即可继承设置） |
| **卸载** | 先托盘退出，再删除整个 `D:\Snipaste` 目录 + 删除上方注册表自启项 |
| **备份设置** | 复制 `config.ini` |
| **清理历史** | 删除 `history\` 目录（会丢失已贴图的图片记录） |

---

## 已知注意事项

- **不要勾选「以管理员身份运行」**（见上文，会破坏开机自启）
- **浏览器内的界面元素检测**需要额外设置：Chrome / Edge 打开 `chrome://accessibility/`，勾选 `Native accessibility API support` 和 `Web accessibility`（关闭浏览器后失效，可用 `--force-renderer-accessibility` 启动参数常驻）
- **`history\`、`crashes\`、`splog.txt` 属于运行时数据**，会随使用不断变化。如果本目录用于 Git 版本管理，建议把它们加入 `.gitignore`，避免每次提交都产生大量二进制改动

---

## 版权与许可

- **Snipaste 是免费软件，但不是开源软件。**
- 版权归 **snipaste.com** 所有：`Copyright (C) 2016-2025 snipaste.com`
- 程序文件（`Snipaste.exe` 及随附的 Qt / OpenSSL / MSVC 运行库等 DLL）**版权归各自作者所有**，本仓库仅为本地留存副本。
- 商业使用请阅读官网的授权说明；另有功能更强的 **Snipaste Pro**。
- Qt 6 采用 LGPL v3，OpenSSL 采用 Apache License 2.0，MSVC 运行库随 Visual Studio 分发条款。

---

*本文档由本地信息核实生成，非 Snipaste 官方文档。官方资料请以 <https://snipaste.com> 与 [GitHub 仓库](https://github.com/Snipaste/feedback) 为准。*
