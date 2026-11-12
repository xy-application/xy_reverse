# 搜索内存特征码 (SearchMemory)

- **支持平台**：`Cross-Platform` (Windows / Linux / macOS)
- **需要管理员权限**：`是` (`requires_admin: true`)

## 功能描述
在目标软件进程或限定模块内存空间中检索特定的特征码或字节序列，支持 `??` 通配符匹配、相对指令寻址（RIP Offset）与指令长度自动修正。

## 运行参数

* **目标进程 (`app`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 目标进程名称、PID 或 AttachSession 标识。
* **驱动模式 (`backend`)**
  * 类型: `string`
  * 默认值: `"auto"`
  * 描述: 内存扫描驱动模式。支持 `"auto"`（推荐自动模式）或 `"native"`。
* **特征码序列 (`pattern`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 十六进制特征码字符串，支持 `??` 通配符（例如 `48 8B 05 ?? ?? ?? ??`）。
* **目标模块 (`module`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 限定搜索的动态链接库模块名称（留空表示搜索目标进程主模块）。
* **相对寻址偏移 (`rip_offset`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 相对偏移计算位置（可选）。
* **指令长度 (`instruction_len`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 完整机器指令长度（可选）。
* **返回全部匹配 (`match_all`)**
  * 类型: `string`
  * 默认值: `"false"`
  * 描述: 是否返回所有匹配地址（`"false"` 仅返回首个匹配地址字符串，`"true"` 返回地址数组）。
* **通道连接配置 (`memflow_connector`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 外部直连或通道配置（硬件物理 DMA / PCIe 直连，普通场景留空）。

## 输出

* 类型: `Mixed`
* 描述: 匹配到的十六进制内存地址字符串（`match_all: false`）或地址列表（`match_all: true`）。若未搜索到则返回空。

## 使用示例

```json
{
  "tag": "SearchMemory",
  "app": "game.exe",
  "backend": "auto",
  "pattern": "48 89 5C 24 ?? 57 48 83 EC",
  "module": "",
  "match_all": "false"
}
```
