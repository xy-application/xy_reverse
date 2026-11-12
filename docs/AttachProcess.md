# 附加进程 (AttachProcess)

- **支持平台**：`Cross-Platform` (Windows / Linux / macOS)
- **需要管理员权限**：`是` (`requires_admin: true`)

## 功能描述
连接并附加到目标软件进程，支持动态加载扩展模块与执行初始化逻辑。附加成功后返回会话 JSON 字符串（包含目标标识与注入会话 ID），可传递给下游操作（如 `CallMethod`）进行方法动态调用。

## 运行参数

* **目标进程 (`app`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 目标进程名称、PID 或完整路径。
* **模块动态库 (`dy`)**
  * 类型: `FilePicker`
  * 默认值: `""`
  * 描述: 待注入或加载的动态链接库文件路径（DLL / SO / DYLIB）。
* **入口函数 (`entrypoint`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 动态库导出入口执行函数名称（可选）。
* **初始化数据 (`data`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 传递给入口函数的初始化配置或载荷字符串。
* **命令行参数 (`command_line`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 可选的附加启动参数（若目标未运行将以挂起状态启动并注入）。

## 输出

* 类型: `string`
* 描述: 注入会话配置 JSON 字符串（包含 `target`, `dy`, `module`, `inject_id`），若失败则抛出错误。

## 使用示例

```json
{
  "tag": "AttachProcess",
  "app": "target_app.exe",
  "dy": "C:/plugins/helper.dll",
  "entrypoint": "InitializePlugin",
  "data": "config_payload"
}
```
