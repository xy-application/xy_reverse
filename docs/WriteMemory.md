# 写入内存 (WriteMemory)

- **支持平台**：`Cross-Platform` (Windows / Linux / macOS)
- **需要管理员权限**：`是` (`requires_admin: true`)

## 功能描述
向目标软件进程或模块内存地址写入数据，支持自动解除页面写保护（VirtualProtect 绕过）、多级指针偏移链寻址以及多种数据格式直接写入。

## 运行参数

* **目标进程 (`app`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 目标进程名称、PID 或 AttachSession 标识。
* **驱动模式 (`backend`)**
  * 类型: `string`
  * 默认值: `"auto"`
  * 描述: 内存写入驱动模式。支持 `"auto"`（推荐自动模式）或 `"native"`。
* **内存地址 (`address`)**
  * 类型: `string`
  * 默认值: `"0x0"`
  * 描述: 十六进制内存地址或基址（例如 `0x7FF710000000`）。
* **数据类型 (`data_type`)**
  * 类型: `string`
  * 默认值: `"i32"`
  * 描述: 写入数据格式。可选基础类型：`i32`, `i64`, `u32`, `u64`, `f32`, `f64`, `string_utf8`, `string_utf16`, `bytes`, `hex`，或者复合结构体定义 `struct: { ... }`。
* **写入数值/内容 (`value`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 待写入的数据数值、文本或十六进制字节串。如果 `data_type` 为 `struct`，此处支持传入 JSON 对象字符串（如 `{"hp": 999, "mp": 500}`），底层将自动二进制打包并单次写入。
* **指针偏移 (`offsets`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 多级指针偏移链（例如 `0x10,0x28`，留空为直接寻址）。
* **自动解除写保护 (`unprotect`)**
  * 类型: `string`
  * 默认值: `"true"`
  * 描述: 写入前是否自动临时修改内存页面保护属性（`"true"`, `"false"`）。
* **通道连接配置 (`memflow_connector`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 外部直连或通道配置（硬件物理 DMA / PCIe 直连，普通场景留空）。

## 输出

* 类型: `bool`
* 描述: 是否成功写入数据到目标内存地址（`1` 表示写入成功）。

## 结构体 Schema 写入 (方案 B 原生支持)

通过在 `data_type` 中声明结构体并在 `value` 中提供 JSON 字段键值，底层将：
1. 校验字段名称与类型；
2. 按照 C 对齐规约自动计算各字段偏移；
3. 将所有字段一次性编译打包为统一二进制 Buffer；
4. 执行**单次原子写操作**，彻底避免多次 IPC 写入造成的数据撕裂与竞态条件。

## 使用示例

### 基础类型写入
```json
{
  "tag": "WriteMemory",
  "app": "game.exe",
  "backend": "auto",
  "address": "0x7FF700100000",
  "data_type": "i32",
  "value": "999",
  "offsets": "0x48,0x10",
  "unprotect": "true"
}
```

### 批量结构体一次性原子写入
```json
{
  "tag": "WriteMemory",
  "app": "game.exe",
  "backend": "auto",
  "address": "0x7FF700100000",
  "data_type": "struct: { hp: i32, mp: i32, x: f32, y: f32 }",
  "value": "{\"hp\": 9999, \"mp\": 5000, \"x\": 100.0, \"y\": 200.0}",
  "offsets": "0x68",
  "unprotect": "true"
}
```

