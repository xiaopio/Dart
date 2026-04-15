在 Dart 中，控制流语句用于根据条件执行不同的代码块或重复执行特定任务。Dart 的语法与 C、Java 和 JavaScript 非常相似。

------

## 1. 选择结构 (Selection Statements)

### **1.1 If 和 Else**

Dart 支持标准的 `if`、`else if` 和 `else`。

- **注意**：条件表达式必须返回布尔值 (`bool`)，Dart 不会自动将其他类型（如数字或字符串）转换为布尔值。

Dart

```dart
var score = 85;

if (score >= 90) {
  print('优秀');
} else if (score >= 60) {
  print('及格');
} else {
  print('不及格');
}
```

### **1.2 Switch 和 Case**

用于多分支判断。

- **Dart 3.0+ 增强**：现在的 `switch` 功能非常强大，支持模式匹配（Pattern Matching）和表达式写法。
- **传统写法**：每个非空 `case` 必须以 `break`、`continue` 或 `return` 结束。

Dart

```dart
var command = 'OPEN';

switch (command) {
  case 'CLOSED':
    print('已关闭');
    break;
  case 'OPEN':
    print('已开启');
    break;
  default:
    print('未知状态');
}
```

- **Switch 表达式 (Dart 3.0+)**：

Dart

```dart
var status = switch (command) {
  'OPEN' => '开启中',
  'CLOSED' => '关闭中',
  _ => '未知', // 下划线表示默认分支
};
```

------

## 2. 循环结构 (Loop Statements)

### **2.1 For 循环**

标准样式的 `for` 循环，适用于已知循环次数的情况。

Dart

```dart
for (var i = 0; i < 5; i++) {
  print('当前索引: $i');
}
```

### **2.2 For-in 循环**

用于遍历实现了 `Iterable` 接口的对象（如 `List` 或 `Set`）。

Dart

```dart
var fruits = ['苹果', '香蕉', '橙子'];
for (var fruit in fruits) {
  print('水果: $fruit');
}
```

### **2.3 While 和 Do-While**

- **While**：先判断条件，再执行。
- **Do-While**：先执行一次，再判断条件。

Dart

```dart
var temp = 10;

// while 循环
while (temp > 0) {
  temp--;
}

// do-while 循环
do {
  print('至少执行一次');
} while (temp > 0);
```

------

## 3. 跳转语句 (Jump Statements)

用于改变循环或选择结构的执行顺序：

- **`break`**：立即跳出当前的循环或 `switch` 结构。
- **`continue`**：跳过当前循环的剩余部分，直接进入下一次循环。

Dart

```dart
for (var i = 0; i < 10; i++) {
  if (i % 2 == 0) continue; // 跳过偶数
  if (i > 7) break;         // 超过7就结束
  print(i); // 输出: 1, 3, 5, 7
}
```

------

## 4. 集合中的选择与循环 (Collection Control Flow)

Dart 允许在构建 `List`、`Set` 或 `Map` 时直接使用 `if` 和 `for`。这在 Flutter 开发中构建组件列表时极其常用。

### **Collection If**

Dart

```dart
var promoActive = true;
var nav = [
  'Home',
  'Furniture',
  if (promoActive) 'Outlet' // 仅当 promoActive 为 true 时添加 'Outlet'
];
```

### **Collection For**

Dart

```dart
var listOfInts = [1, 2, 3];
var listOfStrings = [
  '#0',
  for (var i in listOfInts) '#$i'
];
// 结果: ['#0', '#1', '#2', '#3']
```

------

## 总结

| **语句**                | **适用场景**                       |
| ----------------------- | ---------------------------------- |
| **`if`**                | 处理复杂的逻辑条件判断。           |
| **`switch`**            | 处理单一变量的多个具体离散值判断。 |
| **`for`**               | 标准的计数循环。                   |
| **`for-in`**            | 优雅地遍历集合元素。               |
| **`while`**             | 当满足特定条件时持续运行。         |
| **`Collection if/for`** | 在动态构建集合数据时使用。         |