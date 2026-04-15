Dart 在 2.12 版本引入了**健全的空安全（Sound Null Safety）**。这意味着在编译期间，Dart 就能保证非空类型的变量永远不会为 `null`，从而彻底消除了开发中常见的 `NullPointerException`。

------

## 1. 核心原则：默认不可空

在开启空安全后，所有的类型默认都是**非空**的。如果你声明一个变量但不初始化，或者尝试给它赋值为 `null`，编译器会直接报错。

Dart

```dart
int age = 18;  // 正确
// int count;  // 错误：非空变量必须初始化
// age = null; // 错误：不能将 null 赋值给 int 类型
```

------

## 2. 声明可空类型 (`?`)

如果你确实需要一个变量可以存放 `null`，必须在类型声明后加上问号 `?`。

Dart

```dart
String? name; // name 可以是 String，也可以是 null
name = "Gemini";
name = null; // 合法
```

------

## 3. 操作可空变量

对于可空类型的变量，Dart 提供了几个专门的操作符来确保安全访问：

### **3.1 空断言操作符 (`!`)**

当你百分之百确定一个可空变量在某处不为 `null` 时，可以使用 `!` 强转为非空类型。

> **警告**：如果运行到这里变量确实是 `null`，程序会抛出运行时异常。

Dart

```dart
String? nullableString = "Hello";
String absoluteString = nullableString!; // 强行当作非空使用
```

### **3.2 空安全访问操作符 (`?.`)**

如果对象不为 `null`，则访问成员；如果为 `null`，则直接返回 `null` 而不崩溃。

Dart

```dart
int? length = nullableString?.length; 
```

### **3.3 空合并操作符 (`??`)**

如果左侧表达式为 `null`，则使用右侧的默认值。

Dart

```dart
String displayName = name ?? "游客"; 
```

------

## 4. 延迟初始化 (`late`)

有时候变量确实是非空的，但无法在声明时立即初始化（例如在 `initState` 或构造函数体中赋值）。此时使用 `late` 关键字。

- **延迟赋值**：告诉编译器：“我会稍后初始化它，请别报错。”
- **懒加载**：如果 `late` 变量在赋值前从未被使用，其初始化表达式不会执行。

Dart

```dart
late String description;

void init() {
  description = "这是一段描述";
}
```

------

## 5. 类型收窄 (Type Promotion)

Dart 的编译器非常聪明，如果你通过逻辑判断缩小了变量的范围（例如检查过是否为 `null`），它会自动将可空类型“提升”为非空类型。

Dart

```dart
void printLength(String? text) {
  if (text != null) {
    // 在这个代码块内，text 被自动提升为 String（非空）
    print(text.length); // 不需要使用 ?. 或 !
  }
}
```

------

## 6. 列表与泛型中的空安全

在使用集合时，`?` 的位置决定了什么是可空的：

| **声明方式**       | **含义**                                         |
| ------------------ | ------------------------------------------------ |
| `List<String> a`   | 列表不为 null，里面的字符串也不为 null。         |
| `List<String?> b`  | 列表不为 null，但里面的元素可以是 null。         |
| `List<String>? c`  | 列表可以是 null，但如果它存在，元素必须非 null。 |
| `List<String?>? d` | 列表和元素都可以是 null。                        |

------

## 7. 空安全的好处

1. **更安全**：在编写代码阶段就发现潜在的空值崩溃风险。
2. **更小、更快**：由于 Dart 确定非空变量永远不会为 `null`，编译器可以生成更优化的机器码，减少不必要的判空运行时检查。
3. **代码即文档**：通过类型声明，阅读代码的人一眼就能看出哪些参数是必须的，哪些是可选的。