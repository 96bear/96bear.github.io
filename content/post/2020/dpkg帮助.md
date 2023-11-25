---
title: "dpkg帮助"
date: "2020-07-18"
categories: 
  - "zatan"
---

dpkg 强制选项 - 当遇到问题时控制软件行为： 警告后仍然继续: --force-<手段>,<手段>,... 遇错即停: --refuse-<手段>,<手段>,... | --no-force-<手段>,... 可强制执行的“手段”: \[!\] all 设置所有的强制选项 \[\*\] downgrade 以旧版本的软件包来代替现有软件包 configure-any 配置任何可能与这有关的软件包 hold 处理意外的软件包，即使它处于搁置状态 not-root 以非 root 权限尝试（反）安装这些东西 bad-path 环境变量 PATH 里缺失重要的程序，问题可能 bad-verify 安装一个软件包，即使它的真实性检查失败 bad-version 继续处理，即使软件包版本错误 overwrite 将一个软件包的文件覆盖到另一个包的文件上 overwrite-diverted 以原封未动的文件覆盖一个转移文件 \[!\] overwrite-dir 用其他软件包的文件覆盖一个软件包的目录 \[!\] unsafe-io 不要在解包的时候进行安全I/O操作 \[!\] confnew 总是使用新的配置文件，不提示 \[!\] confold 总是使用旧的配置文件，不提示 \[!\] confdef 当有新的配置文件时使用默认选项，不要提示。 如果找不到默认设置，将会有提示，除非打开 confold 或 confnew 选项 \[!\] confmiss 总是安装缺失的配置文件 \[!\] confask 建议不以新版本配置文件取代原有版本 \[!\] architecture 继续处理，即使软件包没有体系结构或者体系结构错误 \[!\] breaks 继续安装，即使它会损坏另一个软件包 \[!\] conflicts 允许安装存在冲突的软件包 \[!\] depends 将所有依赖问题转为警告 \[!\] depends-version 将所有依赖版本问题转为警告 \[!\] remove-reinstreq 移除要求安装的软件包 \[!\] remove-essential 移除一个至关重要的软件包
