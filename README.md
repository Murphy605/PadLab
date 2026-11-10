# PadLab

PadLab 是一个独立开源项目，目前主要围绕 **EasySMX D10** 的 PC 使用体验、体感输入、设备配置与相关工具进行研究和开发。

当前实际开发重心就是 EasySMX D10。使用 **PadLab** 这个通用名称，只是为了给未来可能加入的其他手柄或控制器项目保留空间，并不代表目前已经是一个多控制器平台。

## 当前项目

### EasySMX D10 PC 工具

正在开发中的 EasySMX D10 PC 端控制与配置工具。

当前目标包括：

- PC 六轴体感控制
- 有线与 2.4G 模式支持
- 三轴硬件校准与六轴校准
- 按键、输入与体感控制
- 配置与游戏 Profile 管理
- Profile 自动切换
- 游戏兼容性测试与适配
- 诊断、日志与测试工具
- Bug 修复与稳定性改进
- 其他与 D10 PC 使用体验相关的功能

### EasySMX D10 Receiver Firmware Patcher

用于处理指定 EasySMX D10 接收器官方固件的独立工具。

PadLab 不提供或重新分发 EasySMX 官方固件。用户需要自行取得对应的官方文件，再由工具进行处理。

公开版本将以独立可执行文件提供。

## 项目原则

- 面向用户的核心功能免费提供
- 项目持续根据真实硬件测试和用户反馈进行改进
- 自动化测试通过不等于真实硬件验证通过
- 实际测试结果、已确认事实、推测和待验证结论应尽量区分
- 不提供或重新分发 EasySMX 官方固件
- 固件相关工具需要用户自行提供对应的官方文件
- PadLab 是独立社区项目，与 EasySMX 无官方关联，也不代表 EasySMX 官方立场

## 开发方式

本项目采用 **AI-assisted engineering（AI 辅助工程）** 的开发方式。

项目方向、需求定义、产品设计、行为取舍、真实硬件测试、验收和发布决策由项目作者负责。

研究、分析、代码实现、测试辅助、工程整理和文档工作中大量使用 AI 工具。

PadLab 不把自动化测试结果直接等同于真实设备结论。涉及手柄、接收器、体感、固件和游戏兼容性的行为，会尽可能通过实际硬件测试进行确认。

## 当前状态

PadLab 目前仍处于早期开发和公开发布准备阶段。

EasySMX D10 的公开版本、下载和完整使用说明将在准备完成后陆续提供。

Bug 反馈与功能建议入口现已开放。

## 社区

### QQ 群

中文社区：**神吹反馈群**

群号：`1131983503`

用于交流 EasySMX D10、PadLab、手柄、体感、游戏、硬件、AI、开发以及其他相关内容，也欢迎临时问题反馈和日常聊天。

需要正式记录和长期追踪的 Bug 或功能建议，请继续使用 GitHub Issues。

本群不是 EasySMX 官方群，PadLab 也不是 EasySMX 官方项目。

## Bug 与功能建议

中文或 English 均可。两种语言使用同一套 Issue 系统，不需要重复提交。

### 🐛 Bug / 问题反馈

如果你遇到了程序启动、设备识别、有线或 2.4G、体感、校准、按键输入、Profile、游戏兼容、界面、固件工具或其他问题：

**[提交 Bug / Report a Bug](https://github.com/Murphy605/PadLab/issues/new?template=bug_report.yml)**

提交时请尽量提供实际确认的信息、复现步骤和相关环境。日志、诊断文件、截图或视频如果能够帮助定位问题，也欢迎一并提供。

### 💡 Feature Request / 功能建议

如果你希望增加新功能、改善现有交互，或者有新的使用场景：

**[提交功能建议 / Request a Feature](https://github.com/Murphy605/PadLab/issues/new?template=feature_request.yml)**

不需要提供代码或完整技术方案。相比单纯提出“增加某个功能”，更希望了解实际使用场景、当前限制以及你希望达到的效果。

### 查看已有问题

提交之前可以先看看是否已经有人提出过类似内容：

**[查看全部 Issues / Browse Issues](https://github.com/Murphy605/PadLab/issues)**

## 反馈与参与

PadLab 是开源项目，但官方主线目前**不接受外部代码贡献**。

欢迎：

- Bug 报告
- 兼容性反馈
- 真实硬件测试结果
- 日志与诊断信息
- 可复现的问题描述
- 功能建议与使用场景
- 协议、USB、HID、IMU、2.4G 等技术观察
- Root cause 分析
- 技术分析与解决思路

如果你已经在自己的 Fork 中解决了某个问题，也欢迎提供问题原因、复现方式、行为变化、测试结果和相关分析。

PadLab 可能参考公开的问题描述、测试结果、观察、技术分析和设计思路，并通过自己的维护流程独立实现解决方案。

外部 Fork 中的代码不会因此自动进入 PadLab 官方主线。

详细说明见：

**[反馈与参与指南](CONTRIBUTING.zh-CN.md)**

## Fork 与修改

PadLab 使用 GPL-3.0-only 开源。

在许可证允许的范围内，你可以：

- Fork 项目
- 阅读和研究源码
- 修改源码
- 自行编译和使用
- 维护自己的版本
- 按 GPL-3.0-only 的要求分发修改版本

私人修改不要求提交回 PadLab。

修改版、Fork 或其他第三方版本不应被描述为 PadLab 官方发布，也不应暗示获得 PadLab 官方认可。

## 许可证

除非另有说明，PadLab 有权授权的原创源代码采用 **GNU General Public License v3.0 only (`GPL-3.0-only`)** 许可。

详见 [LICENSE](LICENSE)。

你可以按照 GPL-3.0-only 的条款使用、研究、修改和分发本项目。

对于不希望受到 GPL 开源义务约束的专有软件、商业产品或 OEM 集成，可另行获得商业授权。商业授权条款将在实际需要时单独协商。

本许可证仅适用于 PadLab 有权授权的原创内容，不对以下材料进行重新许可：

- EasySMX 官方固件
- EasySMX 官方软件
- EasySMX 商标和产品名称
- 其他第三方拥有权利的材料

PadLab 是独立社区项目，与 EasySMX 无官方关联，也不代表 EasySMX 官方立场。

## 项目历史

PadLab 保留项目从早期实验、硬件研究到当前产品化开发过程中的历史记录。

详见：

**[项目历史](HISTORY.zh-CN.md)**

---

# PadLab

PadLab is an independent open-source project currently focused on the **EasySMX D10**, including its PC experience, motion input, device configuration, and related tools.

The actual development focus today is the EasySMX D10. The broader **PadLab** name simply leaves room for possible future controller projects; it does not imply that PadLab is currently a multi-controller platform.

## Current Projects

### EasySMX D10 PC Tools

A PC-side control and configuration project for the EasySMX D10.

Current goals include:

- PC motion control using the controller's IMU
- Wired and 2.4 GHz support
- 3-axis hardware calibration and 6-axis calibration
- Button, input, and motion-control features
- Configuration and game profile management
- Automatic profile switching
- Game compatibility testing and adaptation
- Diagnostics, logging, and testing tools
- Bug fixes and stability improvements
- Other improvements to the D10 PC experience

### EasySMX D10 Receiver Firmware Patcher

A standalone tool for processing a specific official EasySMX D10 receiver firmware package.

PadLab does not provide or redistribute official EasySMX firmware. Users must obtain the corresponding official file themselves before processing it with the tool.

The public version will be provided as a standalone executable.

## Project Principles

- Core user-facing features will remain free
- Development is driven by real hardware testing and user feedback
- Passing automated tests does not automatically mean physical hardware validation has passed
- Confirmed facts, actual test results, hypotheses, and unverified conclusions should be distinguished whenever possible
- Official EasySMX firmware is not included or redistributed
- Firmware-related tools require users to provide the corresponding official files themselves
- PadLab is an independent community project and is not affiliated with or endorsed by EasySMX

## Development Approach

This project uses an **AI-assisted engineering** workflow.

Project direction, requirements, product design, behavioral decisions, physical hardware testing, acceptance, and release decisions are handled by the project author.

AI tools are extensively used to assist with research, analysis, implementation, testing, engineering work, and documentation.

PadLab does not treat automated test results as automatic proof of real-device behavior. Controller, receiver, motion-control, firmware, and game-compatibility behavior is validated on physical hardware whenever possible.

## Current Status

PadLab is currently in early development and public-release preparation.

Public EasySMX D10 releases, downloads, and complete usage documentation will be provided as they become ready.

Bug reporting and feature-request channels are now open.

## Community

### Discord

Join the PadLab community for discussion about the EasySMX D10, motion control, controllers, games, hardware, development, and related topics.

**[Join the PadLab Discord](https://discord.gg/dEBXrFhEK8)**

Casual troubleshooting and discussion are welcome on Discord.

For bugs and feature requests that need formal tracking, please use GitHub Issues.

## Bugs and Feature Requests

Chinese or English is welcome. Both languages use the same Issue system, so there is no need to submit the same report twice.

### 🐛 Bug Report

If you encounter problems involving startup, device detection, wired or 2.4 GHz operation, motion control, calibration, button input, profiles, game compatibility, UI, firmware tools, or anything else:

**[Report a Bug / 提交 Bug](https://github.com/Murphy605/PadLab/issues/new?template=bug_report.yml)**

Please provide information you have actually confirmed, along with reproduction steps and relevant environment details whenever possible. Logs, diagnostic files, screenshots, and videos are also welcome when they help identify the issue.

### 💡 Feature Request

If you would like to suggest a new feature, improve an existing interaction, or describe a new use case:

**[Request a Feature / 提交功能建议](https://github.com/Murphy605/PadLab/issues/new?template=feature_request.yml)**

You do not need to provide code or a complete technical design. Rather than only saying "add feature X", it is more useful to explain the real-world use case, current limitation, and desired outcome.

### Existing Issues

Before submitting, you may want to check whether a similar issue already exists:

**[Browse Issues / 查看全部 Issues](https://github.com/Murphy605/PadLab/issues)**

## Feedback and Participation

PadLab is open source, but the official mainline currently **does not accept external code contributions**.

The following are welcome:

- Bug reports
- Compatibility feedback
- Physical hardware test results
- Logs and diagnostic information
- Reproducible problem reports
- Feature requests and use cases
- Technical observations involving protocols, USB, HID, IMU, 2.4 GHz, and related areas
- Root-cause analysis
- Technical analysis and possible solution directions

If you have already fixed an issue in your own fork, you are welcome to share the underlying problem, reproduction steps, behavioral changes, test results, and technical analysis.

PadLab may use publicly shared problem descriptions, test results, observations, technical analysis, and design ideas, while independently implementing solutions through its own maintenance workflow.

Code from external forks does not automatically enter the official PadLab mainline.

See:

**[Feedback and Participation Guide](CONTRIBUTING.md)**

## Forking and Modification

PadLab is open source under GPL-3.0-only.

Within the terms of the license, you may:

- Fork the project
- Read and study the source code
- Modify the source code
- Build and use your own version
- Maintain your own fork
- Distribute modified versions in compliance with GPL-3.0-only

Private modifications do not need to be contributed back to PadLab.

Modified versions, forks, and other third-party builds must not be presented as official PadLab releases or imply official endorsement by PadLab.

## License

Unless otherwise stated, original material that PadLab has the right to license is released under the **GNU General Public License v3.0 only (`GPL-3.0-only`)**.

See [LICENSE](LICENSE).

You may use, study, modify, and distribute this project under the terms of GPL-3.0-only.

Separate commercial licensing may be available for proprietary software, commercial products, or OEM integrations that do not wish to operate under the GPL requirements. Commercial terms will be negotiated separately when needed.

This license applies only to original material that PadLab has the right to license. It does not relicense:

- Official EasySMX firmware
- Official EasySMX software
- EasySMX trademarks or product names
- Other third-party material

PadLab is an independent community project and is not affiliated with or endorsed by EasySMX.

## Project History

PadLab preserves the history of the project from its early experiments and hardware research through the current product-development phase.

See:

**[Project History](HISTORY.md)**
