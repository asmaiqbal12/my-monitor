# 📷 实验室智能监控系统 (Lab Security Monitor)

  <p align="center">
    Claude Code 和 Google Gemini 辅助创建的：一个基于 Python 和 OpenCV 的智能视频监控系统。专为实验室等环境设计，可利用电脑摄像头实现开关门等运动检测。具备实时弹窗提醒、自动截图记录、现代化UI界面、系统托盘运行等功能。
  </p>

  <p align="center">
    <a href="https://github.com/Lecr7s/MyMonitor">
      <img src="https://img.shields.io/badge/version-v3.0_Pro-brightgreen" alt="Version">
    </a>
    <img src="https://img.shields.io/badge/Python-3.8+-blue?logo=python&logoColor=white" alt="Python">
    <img src="https://img.shields.io/badge/OpenCV-4.x-red?logo=opencv&logoColor=white" alt="OpenCV">
    <img src="https://img.shields.io/badge/UI-CustomTkinter-blue" alt="UI">
    <img src="https://img.shields.io/badge/Platform-Windows-0078D6?logo=windows&logoColor=white" alt="Platform">
    <img src="https://img.shields.io/badge/license-MIT-green" alt="License">
  </p>

  <p align="center">
    <a href="#-功能特性">功能特性</a> •
    <a href="#-系统要求">系统要求</a> •
    <a href="#-安装指南">安装指南</a> •
    <a href="#-使用说明">使用说明</a> •
    <a href="#-配置详解">配置详解</a> •
    <a href="#-常见问题-troubleshooting">常见问题</a> •
    <a href="#-如何打包软件">打包软件</a>
  </p>
</div>

---

## ✨ 功能特性

*   **🎥 实时运动检测**: 使用高斯模糊和帧差法检测画面变化，有效识别移动物体。
*   **🛡️ 智能防抖**: 可配置`continuous_frames`（连续检测帧数），只有连续多帧检测到运动才触发报警，大幅降低误报率。
*   **📸 自动抓拍**: 触发报警时自动保存截图（默认连拍3张），并记录日志。
*   **💾 预设管理**: 支持保存和加载多组灵敏度参数，适应不同光照和环境。
*   **🔽 最小化后台运行**: 支持最小化到系统托盘，后台静默监控，不干扰日常工作。
*   **🌘 深色模式 UI**: 基于 CustomTkinter 的现代化用户界面，美观且护眼。
*   **🧹 自动清理**: 内置截图自动清理功能，防止磁盘空间占满。

---

## 🛠️ 系统要求

*   **操作系统**: Windows 10/11 (推荐)
*   **运行环境**: Python 3.8 或更高版本
*   **硬件**: USB 摄像头或笔记本内置摄像头

---

## 🚀 安装指南

1.  **克隆仓库**:
    ```bash
    git clone https://github.com/lecrix/my-monitor.git
    cd my-monitor
    ```

2.  **安装依赖**:
    ```bash
    pip install -r requirements.txt
    ```

---

## 📖 使用说明

### 启动程序
```bash
python monitor.py
```

### 快捷键操作
| 快捷键         | 功能                            |
| :------------- | :------------------------------ |
| `Space`        | 启动 / 暂停监控                 |
| `Ctrl + S`     | 手动抓拍当前画面                |
| `Ctrl + R`     | 重设监控区域 (ROI)              |
| `Ctrl + 1/2/3` | 快速切换预设方案 (需先保存预设) |
| `Esc`          | 退出 ROI 选择模式               |

### 界面功能
*   **ROI 选择**: 点击"重设区域"可框选重点监控区域（如门口、仪器），排除不相关区域的干扰。
*   **灵敏度调节**: 界面右侧实时调节阈值，数值越小越灵敏。
*   **截图查看**: 点击"相册"按钮直接打开截图保存文件夹。

---

## ⚙️ 配置详解

程序首次运行后会在根目录生成 `config.json`。你可以通过界面修改，也可以手动编辑此文件。

### 核心参数
| 参数名              | 默认值 | 说明                                                                             |
| :------------------ | :----- | :------------------------------------------------------------------------------- |
| `camera_id`         | `0`    | 摄像头ID，默认0为第一个摄像头。如果有多个摄像头，可改为1, 2等。                  |
| `min_area`          | `500`  | **灵敏度阈值**。检测到的运动物体面积（像素²）。数值越小越灵敏，越容易报警。      |
| `threshold`         | `25`   | **二值化阈值**。判断像素变化的差异标准。数值越小，对光线变化越敏感。             |
| `continuous_frames` | `3`    | **防抖帧数**。必须连续检测到运动多少帧才触发报警。防止虫子飞过或闪光造成的误报。 |
| `alert_cooldown`    | `3`    | **报警冷却(秒)**。两次报警之间的最小间隔，防止日志刷屏。                         |

### 截图与存储
| 参数名                 | 默认值 | 说明                                                 |
| :--------------------- | :----- | :--------------------------------------------------- |
| `screenshot_count`     | `3`    | 报警时连拍张数。                                     |
| `screenshot_interval`  | `0.5`  | 连拍间隔时间(秒)。                                   |
| `cleanup_days`         | `3`    | 截图保留天数。超过此天数的截图将在启动时被自动清理。 |
| `auto_cleanup_enabled` | `true` | 是否启用自动清理功能。                               |

### 高级设置
| 参数名              | 默认值 | 说明                                                              |
| :------------------ | :----- | :---------------------------------------------------------------- |
| `loop_delay`        | `0.2`  | 视频循环延迟(秒)。决定了检测频率。0.2秒约等于5FPS，适合后台运行。 |
| `gaussian_blur`     | `21`   | 高斯模糊核大小，用于去除噪点。必须是奇数。                        |
| `dilate_iterations` | `2`    | 膨胀迭代次数，用于补全检测到的物体边缘。                          |

---

## ❓ 常见问题 (Troubleshooting)

### Q1: 启动时提示 "无法连接摄像头"
*   检查摄像头是否被其它程序（如Zoom, 腾讯会议）占用。
*   尝试修改 `config.json` 中的 `camera_id` 为 `1` 或 `2`。
*   拔插 USB 摄像头重试。

### Q2: 误报太多，一直响
*   **增大 `min_area`**: 比如调整到 1000 或 2000，只检测大物体（人）。
*   **增大 `continuous_frames`**: 调整到 5 或 10，过滤短暂干扰。
*   使用 **ROI** 功能，避开会有光影变化的区域（如窗帘、闪烁的LED灯）。

### Q3: 程序运行报错 `ImportError`
*   请确认已运行 `pip install -r requirements.txt`。
*   如果使用 Anaconda，请确保在正确的环境中运行。

---

## 📦 如何打包软件

如果你想将 Python 脚本打包成无需安装环境即可运行的 `.exe` 文件，请按照以下步骤操作：

1.  **安装 PyInstaller**:
    ```bash
    pip install pyinstaller
    ```

2.  **执行打包命令**:
    *   **方法 A (使用现有配置)** - *推荐*:
        项目包含已配置好的 `main.spec` 文件，直接运行即可：
        ```bash
        pyinstaller main.spec
        ```
    *   **方法 B (手动打包)**:
        如果无法使用 spec 文件，可以使用以下命令：
        ```bash
        pyinstaller -F -w -i cctv.ico monitor.py
        ```
        *   `-F`: 打包成单个 exe 文件
        *   `-w`: 运行时不显示黑色控制台窗口
        *   `-i cctv.ico`: 指定程序图标

3.  **获取软件**:
    打包完成后，在 `dist/` 目录下可以找到 `MyMonitor.exe` (或 `monitor.exe`)。

---

## 📂 目录结构说明

```text
my-monitor/
├── cctv.ico                 # 应用程序图标
├── monitor.py               # 主程序入口
├── config.json              # 用户配置文件 (自动生成)
├── window_layout.json       # 窗口布局记忆 (自动生成)
├── security_monitor.log     # 运行日志
├── screenshots/             # [目录] 所有的报警截图
├── requirements.txt         # 依赖说明
├── README.md                # 说明文档
└── LICENSE                  # 许可证
```

---

## 📄 许可证

本项目采用 [MIT 许可证](LICENSE)。

---

## 🎁 致谢

### 技术支持
*   [OpenCV](https://opencv.org/) - 强大的计算机视觉库
*   [CustomTkinter](https://github.com/TomSchimansky/CustomTkinter) - 现代化的 Python UI 库
*   [Pillow](https://python-pillow.org/) - Python 图像处理库
*   [Pystray](https://github.com/moses-palmer/pystray) - 系统托盘图标支持

### 灵感来源
*   Github 开源社区的优秀项目
*   实验室安全监控的实际需求

### 特别感谢
*   **开源社区** - 全球开发者的无数贡献
*   **Claude Code & Google Gemini** - 高效的 AI 代码辅助
*   **你** - 感谢使用本项目！

---

## 📮 联系与反馈

### 需要帮助？
1.  查看 [常见问题](#-常见问题-troubleshooting) 章节
2.  检查程序运行日志 `security_monitor.log`
3.  如有 Bug，请在 GitHub Issues 中反馈

### 想要贡献？
欢迎提交 Pull Request！
*   提交代码前请确保通过测试
*   请保持代码风格一致

---

<div align="center">
  <h3>🌟 如果这个项目对你有帮助，请给个 Star 吧！ 🌟</h3>
  
  <p>Made with ❤️ and Claude Code & Gemini</p>

</div>

---

<div align="center">
  版本: v3.0 更新日期: 2025-12-10 状态: ✅ 稳定版本
</div>
