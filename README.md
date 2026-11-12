# 🔬 底层调试与内存分析探针套件 (Native Debug & Memory Inspector)

[![Plugin Version](https://img.shields.io/badge/version-0.54.0-blue.svg)](manifest.json)
[![Group](https://img.shields.io/badge/group-Reverse-purple.svg)](#)
[![Platform](https://img.shields.io/badge/platform-All-green.svg)](#)
[![License](https://img.shields.io/badge/license-Freeware-brightgreen.svg)](#)
[![Discord](https://img.shields.io/discord/832082456727257118?label=Discord&logo=discord&logoColor=white&color=5865F2)](https://discord.gg/g5Fh3FtfGx)

专为底层逆向工程、安全诊断、进程钩子注入、动态分析及游戏与原生程序测试设计的专家级插件套件。提供目标进程挂载与动态库注入（Frida 驱动）、导出函数跨进程/本地安全调用、操作系统底层句柄检索与强制释放、虚拟/物理DMA多级指针内存读写、自动解除页面写保护以及特征码（AOB）相对寻址扫描。

---

## 📖 简介 (Overview)

`xy_reverse` 提供了跨平台的专家级底层系统探测与分析能力，包含 6 项高内聚核心动作：
- **进程附加与模块注入 (`AttachProcess`)**：基于 Frida 动态附加目标进程或以挂起状态启动进程并注入指定的动态链接库（DLL / SO / DYLIB），支持导出入口函数执行与初始化参数传递，返回可串联的会话标识。
- **导出函数动态调用 (`CallMethod`)**：调用目标进程已加载模块或本地模块中的指定导出函数，支持 `default`、`stdcall`、`fastcall`、`thiscall` 等调用约定与多类型参数动态打包，无缝承接 `AttachProcess` 会话。
- **系统句柄检索与释放 (`CloseHandle`)**：枚举并强制关闭目标进程占用的文件锁、互斥体（Mutex）、信号量、事件或进程句柄，解决程序多开互斥限制或被锁定的资源释放。
- **虚拟与DMA物理内存读取 (`ReadMemory`)**：按地址或多级指针偏移链（如 `0x10,0x28`）读取目标进程虚拟内存或 PCIe DMA 硬件物理内存，支持整型、浮点、UTF-8/UTF-16 字符串、二进制字节数组及十六进制格式。
- **内存特征码扫描 (`SearchMemory`)**：在目标进程或指定模块内存空间中高效检索特征码序列（支持 `??` 通配符），支持相对寻址偏移计算（RIP Offset）与指令长度自动修正。
- **内存数据写入与写保护解除 (`WriteMemory`)**：向目标进程指定内存地址或多级指针链写入数据，内置自动页面写保护检测与解除机制（VirtualProtect），支持多类型格式与硬件 DMA 直连通道。

---

## 🛠️ 动作指令全景速查 (Action Catalog)

| 动作标识 (Tag) | 功能名称 | 描述 | 支持平台 | 提权需求 |
| :--- | :--- | :--- | :--- | :--- |
| `AttachProcess` | 附加与模块注入 | 连接并附加到目标软件进程，支持动态加载扩展模块与执行初始化逻辑 | `Cross-Platform` | `true` |
| `CallMethod` | 调用导出方法 | 调用目标进程加载模块中的指定导出函数并获取执行结果 | `Cross-Platform` | `false` |
| `CloseHandle` | 关闭系统句柄 | 释放已打开的系统资源句柄，解除互斥体与文件锁占用 | `Cross-Platform` | `true` |
| `ReadMemory` | 读取进程内存 | 读取目标软件进程或模块内存地址中的数据，支持多种数据类型与偏移链解析 | `Cross-Platform` | `true` |
| `SearchMemory` | 搜索内存特征码 | 在目标软件进程或模块内存中检索特定的特征码或字节序列（AOB） | `Cross-Platform` | `true` |
| `WriteMemory` | 写入进程内存 | 向目标软件进程或模块内存地址写入数据，支持自动解除页面写保护 | `Cross-Platform` | `true` |

---

## 📄 许可证 (License)

本插件遵循免费专有许可协议 (Freeware)。供 小友+ 用户免费下载与使用，未经官方书面授权，严禁对二进制文件进行逆向工程、反编译或二次打包转售。
