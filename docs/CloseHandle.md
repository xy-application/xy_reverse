# 关闭系统句柄 (CloseHandle)

- **支持平台**：`Cross-Platform` (Windows / Linux / macOS)
- **需要管理员权限**：`是` (`requires_admin: true`)

## 功能描述
释放已打开的系统资源句柄，包括文件、互斥体、信号量、事件、进程与线程句柄等，解决多开冲突或独占锁占用问题，保证系统资源稳定回收。

## 运行参数

* **句柄类型 (`type`)**
  * 类型: `string`
  * 默认值: `"Process"`
  * 描述: 系统句柄资源类型。可选值：`Process`, `Thread`, `File`, `Event`, `Mutex`, `Semaphore`, `Key`, `All`。
* **句柄名称或关键字 (`name`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 待释放句柄的名称、标识或过滤匹配关键字（支持通配符 `*` 与 `?`）。
* **关联进程文件 (`exe_file`)**
  * 类型: `FilePicker`
  * 默认值: `""`
  * 描述: 关联的可执行程序文件（可选），用于过滤特定程序占用的句柄。

## 输出

* 类型: `bool`
* 描述: 是否成功匹配并关闭目标句柄资源（`1` 表示成功释放，`0` 表示未找到匹配句柄）。

## 使用示例

```json
{
  "tag": "CloseHandle",
  "type": "File",
  "name": "*\\DiabloII Check For Other Instances",
  "exe_file": "D:/Games/D2R/D2R.exe"
}
```
