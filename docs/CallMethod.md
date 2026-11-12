# 调用导出方法 (CallMethod)

- **支持平台**：`Cross-Platform` (Windows / Linux / macOS)
- **需要管理员权限**：`否` (`requires_admin: false`)

## 功能描述
调用目标进程加载模块或本地动态链接库中的指定导出函数并获取执行结果。支持多平台原生调用约定与参数动态类型转换（整数、浮点数、布尔值、字符串、指针等），可与 `AttachProcess` 产出的会话对象无缝串联。

## 运行参数

* **模块/会话对象 (`object`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 目标模块名称、动态链接库路径或 `AttachProcess` 导出的会话 JSON 字符串。
* **方法名称 (`method`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 待调用的导出函数名称。
* **返回值类型 (`return_type`)**
  * 类型: `string`
  * 默认值: `"void"`
  * 描述: 函数返回值的数据解析类型。可选值：`int`, `uint`, `string`, `pointer`, `bool`, `void`, `float`。
* **调用约定 (`abi`)**
  * 类型: `string`
  * 默认值: `"default"`
  * 描述: 底层函数调用约定。可选值：`default`, `stdcall`, `fastcall`, `thiscall`。
* **动态参数 (`params`)**
  * 类型: `Array`
  * 默认值: `[]`
  * 描述: 按顺序传递给导出函数的参数列表，支持工具栏增删与上下移动调整。

## 输出

* 类型: `Mixed`
* 描述: 目标函数执行返回的结果值（按 `return_type` 解析）。

## 使用示例

```json
{
  "tag": "CallMethod",
  "object": "helper.dll",
  "method": "GetStatus",
  "return_type": "int",
  "abi": "stdcall"
}
```
