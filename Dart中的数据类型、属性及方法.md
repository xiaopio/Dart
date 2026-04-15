在 Dart 语言中，所有数据类型都是**对象**，它们继承自 `Object` 类。Dart 的类型系统非常完善，包含基本类型、集合类型以及一些特殊类型。

------

## 1. 数值类型 (Numbers)

数值类型包括整数 `int` 和浮点数 `double`，它们都继承自 `num`。

### **int & double (num)**

- **常用属性：**
  - `isEven` / `isOdd`: 判断是否为偶数/奇数（仅限 `int`）。
  - `isNaN`: 是否为非数字。
  - `isNegative`: 是否为负数。
  - `sign`: 返回数值符号（-1, 0, 1）。
- **常用方法：**
  - `abs()`: 返回绝对值。
  - `ceil()` / `floor()`: 向上/向下取整。
  - `round()`: 四舍五入。
  - `toDouble()` / `toInt()`: 类型转换。
  - `toStringAsFixed(n)`: 保留 n 位小数并转为字符串。
  - `parse(string)`: 将字符串解析为数值。

------

## 2. 字符串 (Strings)

`String` 类型用于表示文本，采用 UTF-16 编码。

### **String**

- **常用属性：**
  - `length`: 字符串长度。
  - `isEmpty` / `isNotEmpty`: 是否为空。
  - `codeUnits`: 返回 UTF-16 代码单元列表。
- **常用方法：**
  - `substring(start, [end])`: 截取子串。
  - `contains(pattern)`: 是否包含指定内容。
  - `split(pattern)`: 分割字符串。
  - `toLowerCase()` / `toUpperCase()`: 大小写转换。
  - `trim()` / `trimLeft()` / `trimRight()`: 去除空格。
  - `replaceAll(from, replace)`: 全局替换。
  - `indexOf(pattern)`: 查找索引。

------

## 3. 布尔类型 (Booleans)

`bool` 类型只有两个值：`true` 和 `false`。

### **bool**

- **常用操作：**
  - 主要用于逻辑运算（`&&`, `||`, `!`）。
  - `toString()`: 转为字符串 "true" 或 "false"。

------

## 4. 集合类型 (Collections)

Dart 提供了三种核心集合：`List`（列表）、`Set`（集合）、`Map`（键值对）。

### **List (有序列表)**

- **常用属性：**
  - `length`: 长度。
  - `reversed`: 返回倒序迭代器。
  - `first` / `last`: 获取第一个/最后一个元素。
- **常用方法：**
  - `add(value)` / `addAll(iterable)`: 添加元素。
  - `insert(index, value)`: 插入元素。
  - `remove(value)` / `removeAt(index)`: 删除元素。
  - `indexOf(value)`: 查找索引。
  - `forEach(callback)`: 遍历。
  - `map(callback)`: 映射转换。
  - `where(callback)`: 过滤。
  - `sort()`: 排序。

### **Set (无序且唯一)**

- **常用属性：**
  - `length`: 元素个数。
- **常用方法：**
  - `add(value)`: 添加元素（若已存在则无效）。
  - `contains(value)`: 检查是否存在。
  - `intersection(other)`: 交集。
  - `union(other)`: 并集。
  - `difference(other)`: 差集。

### **Map (键值对)**

- **常用属性：**
  - `keys`: 获取所有键。
  - `values`: 获取所有值。
  - `length`: 键值对数量。
- **常用方法：**
  - `addAll(otherMap)`: 合并 Map。
  - `containsKey(key)` / `containsValue(value)`: 检查键或值是否存在。
  - `remove(key)`: 根据键删除。
  - `putIfAbsent(key, callback)`: 如果键不存在则添加。

------

## 5. 符号与字符 (Symbols & Runes)

- **Runes**: 用于表示字符串中的 UTF-32 字符（常用于处理 Emoji 或特殊符号）。
- **Symbol**: 用于表示操作符或标识符，通常在反射（mirrors）中使用。

------

## 6. 特殊类型 (Special Types)

| **类型**  | **描述**                                                 |
| --------- | -------------------------------------------------------- |
| `Object`  | 所有非空类型的基类。                                     |
| `dynamic` | 动态类型，关闭编译时类型检查（慎用）。                   |
| `var`     | 类型推断，一旦赋值后类型固定。                           |
| `final`   | 只能赋值一次的变量。                                     |
| `const`   | 编译时常量。                                             |
| `Never`   | 表示表达式永远不会成功完成（例如总是抛出异常的函数）。   |
| `void`    | 表示函数没有返回值。                                     |
| `Null`    | 唯一的值是 `null`（开启空安全后需用 `Type?` 表示可空）。 |

------

## 7. 记录类型 (Records) - Dart 3.0+

Dart 3.0 引入的新类型，允许聚合多个对象。

- **示例：** `var record = (1, 'a', b: true);`
- **访问方式：**
  - `record.$1`: 访问第一个位置字段。
  - `record.b`: 访问命名字段。

------

## 8. 枚举 (Enums)

用于定义固定数量的常量。

- **常用属性：**
  - `index`: 获取枚举值的索引。
  - `name`: 获取枚举值的字符串名称。
  - `values`: 获取枚举类中所有值的列表。