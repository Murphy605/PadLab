# PadLab

一个面向游戏手柄、体感输入与控制器相关工具的独立开源项目。

PadLab 主要用于研究和开发 PC 手柄相关的软件、体感控制、设备配置、固件工具以及其他硬件实验项目。

当前首个主要项目围绕 **EasySMX D10** 展开。

## 当前项目

### EasySMX D10 PC 工具

正在开发中的 D10 PC 端控制与配置工具，目标包括：

- PC 六轴体感控制
- 有线与 2.4G 模式支持
- 六轴与硬件校准
- 配置与游戏 Profile 管理
- 兼容性测试与 Bug 修复
- 其他与 D10 PC 使用体验相关的功能

### EasySMX D10 Receiver Firmware Patcher

用于处理指定 EasySMX D10 接收器官方固件的独立工具。

公开版本将以单独的可执行文件发布。

## 项目原则

- 面向用户的核心功能免费提供
- 项目持续根据真实硬件测试和用户反馈进行改进
- 不提供或重新分发 EasySMX 官方固件
- 固件相关工具需要用户自行提供对应的官方文件
- 项目与 EasySMX 无官方关联，也不代表 EasySMX 官方立场

## 开发方式

本项目采用 **AI-assisted engineering（AI 辅助工程）** 的开发方式。

项目方向、产品设计、真实硬件测试、行为验收和发布决策由项目作者负责；研究、代码实现、分析与工程整理过程中大量使用 AI 工具辅助完成。

## 状态

PadLab 目前仍处于早期开发阶段。

EasySMX D10 的公开版本、下载、使用说明和 Bug 反馈入口将在准备完成后陆续开放。

## 许可证

除非另有说明，PadLab 原创源代码采用 **GNU General Public License v3.0 only (`GPL-3.0-only`)** 许可。详见 [LICENSE](LICENSE)。

你可以按照 GPL-3.0-only 的条款自由使用、研究、修改和分发本项目。

对于不希望受到 GPL 开源义务约束的专有软件、商业产品或 OEM 集成，可另行获得商业授权。商业授权条款将在实际需要时单独协商。

本许可证仅适用于 PadLab 有权授权的原创内容，不对 EasySMX 官方固件、官方软件、商标、产品名称或其他第三方材料进行重新许可。

PadLab 是独立社区项目，与 EasySMX 无官方关联，也不代表 EasySMX 官方立场。


完整开发过程见 [项目历史](HISTORY.zh-CN.md)。

---

# PadLab

An independent open-source project focused on game controllers, motion input, controller utilities, and hardware experiments.

PadLab is used to research and develop PC controller software, motion-control tools, device configuration utilities, firmware-related tools, and other controller-focused projects.

The first major project currently focuses on the **EasySMX D10**.

## Current Projects

### EasySMX D10 PC Tools

A PC-side control and configuration project for the EasySMX D10.

Current goals include:

- PC motion control using the controller's IMU
- Wired and 2.4 GHz support
- Motion and hardware calibration
- Configuration and game profile management
- Compatibility testing and bug fixes
- Other improvements to the D10 PC experience

### EasySMX D10 Receiver Firmware Patcher

A standalone tool for processing a specific official EasySMX D10 receiver firmware package.

The public version will be distributed as a standalone executable.

## Project Principles

- Core user-facing features will remain free
- Development is driven by real hardware testing and user feedback
- Official EasySMX firmware is not included or redistributed
- Firmware-related tools require users to provide the corresponding official files themselves
- This is an unofficial community project and is not affiliated with or endorsed by EasySMX

## Development Approach

This project uses an **AI-assisted engineering** workflow.

Project direction, product design, physical hardware testing, behavioral validation, and release decisions are handled by the project author. AI tools are extensively used to assist with research, implementation, analysis, and engineering work.

## Status

PadLab is currently in early development.

Public EasySMX D10 releases, downloads, documentation, and bug-reporting channels will be added as they become ready.

## License

Unless otherwise stated, original PadLab source code is licensed under the **GNU General Public License v3.0 only (`GPL-3.0-only`)**. See [LICENSE](LICENSE).

You may use, study, modify, and distribute this project under the terms of GPL-3.0-only.

Separate commercial licensing may be available for proprietary software, commercial products, or OEM integrations that do not wish to operate under the GPL requirements. Commercial terms will be negotiated separately when needed.

This license applies only to original material that PadLab has the right to license. It does not relicense official EasySMX firmware, software, trademarks, product names, or other third-party materials.

PadLab is an independent community project and is not affiliated with or endorsed by EasySMX.


See [Project History](HISTORY.md) for the full development story.
