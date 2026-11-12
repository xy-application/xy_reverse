# 读取内存 (ReadMemory)

- **支持平台**：`Cross-Platform` (Windows / Linux / macOS)
- **需要管理员权限**：`是` (`requires_admin: true`)

## 功能描述
读取目标软件进程或模块内存地址中的数据，支持多种底层数据格式（整数、浮点、字符串、字节序列）与多级指针偏移链解析，并支持直连或硬件物理 DMA 通道读取。

## 运行参数

* **目标进程 (`app`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 目标进程名称、PID 或 AttachSession 标识。
* **驱动模式 (`backend`)**
  * 类型: `string`
  * 默认值: `"auto"`
  * 描述: 内存访问驱动模式。支持 `"auto"`（推荐自动模式）或 `"native"`。
* **内存地址 (`address`)**
  * 类型: `string`
  * 默认值: `"0x0"`
  * 描述: 十六进制内存地址或基址（例如 `0x7FF710000000`）。
* **数据类型 (`data_type`)**
  * 类型: `string`
  * 默认值: `"i32"`
  * 描述: 读取解析的数据格式。可选基础类型：`i32`, `i64`, `u32`, `u64`, `f32`, `f64`, `string_utf8`, `string_utf16`, `bytes`, `hex`，或者复合结构体定义 `struct: { ... }`（单次 IPC 读取整个结构体并解析为 JSON 字典）。
* **读取长度 (`size`)**
  * 类型: `string`
  * 默认值: `"4"`
  * 描述: 读取字节数（适用于 bytes / hex / string；若为 struct，将自动根据结构体对齐布局精确计算总尺寸）。
* **指针偏移 (`offsets`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 多级指针偏移链（例如 `0x10,0x28`，留空为直接寻址）。
* **通道连接配置 (`memflow_connector`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 外部直连或通道配置（硬件物理 DMA / PCIe 直连，普通场景留空）。

## 输出

* 类型: `Mixed`
* 描述: 解析后得到的数值、文本字符串、十六进制字节串或结构体解析出的 JSON 字典对象字符串。

## 结构体 Schema 语法 (方案 B 原生支持)

通过在 `data_type` 中传入 `struct: { ... }`，底层将自动按 C 对齐规则计算字段偏移，执行**单次原子 IPC 内存读取**，极大降低系统调用开销：

1. **C 风格语法**：
   `"data_type": "struct: { int hp, int mp, float x, float y, float z }"`
2. **键值语法**：
   `"data_type": "struct: { hp: i32, mp: i32, x: f32, y: f32 }"`
3. **显式内存偏移与定长字符串**：
   `"data_type": "struct: { 0x00: hp: i32, 0x04: mp: i32, 0x10: name: string[32] }"` 或 `"struct: { hp: i32 @ 0x00, name: string[32] @ 0x10 }"`
4. **紧凑模式 (Packed)**：
   `"data_type": "struct packed: { byte flags, int id }"`

## 使用示例

### 基础类型读取
```json
{
  "tag": "ReadMemory",
  "app": "game.exe",
  "backend": "auto",
  "address": "0x7FF700100000",
  "data_type": "i32",
  "size": "4",
  "offsets": "0x48,0x10"
}
```

### 批量结构体一次性读取
```json
{
  "tag": "ReadMemory",
  "app": "game.exe",
  "backend": "auto",
  "address": "0x7FF700100000",
  "data_type": "struct: { hp: i32, mp: i32, x: f32, y: f32, name: string[16] }",
  "offsets": "0x68"
}
```
**输出结果**：
```json
"{\"hp\": 100, \"mp\": 50, \"name\": \"Player1\", \"x\": 12.5, \"y\": 34.0}"
```

