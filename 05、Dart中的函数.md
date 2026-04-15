在 Dart 中，函数是**一等公民（First-class objects）**，这意味着函数可以作为变量存储、作为参数传递或作为其他函数的返回值。

------

## 1. 函数定义与组成

一个标准的函数由返回值、函数名、参数列表和函数体组成。

Dart

```dart
returntype functionName(parameters) {
  // 函数体
  return value;
}
```

- **类型推导**：如果不指定返回值类型，Dart 会自动推导，但建议显式声明以提高代码可读性。

- **箭头函数**：对于只包含一个表达式的函数，可以使用 `=>` 简写。

  Dart

  ```dart
  bool isNoble(int atomicNumber) => atomicNumber > 0;
  ```

------

## 2. 参数类型

Dart 的参数处理非常灵活，主要分为**可选参数**和**必需参数**。

### **2.1 位置参数 (Positional Parameters)**

- **必需位置参数**：按顺序传入。

- **可选位置参数**：使用 `[]` 包裹，调用时可省略。

  Dart

  ```dart
  String say(String from, String msg, [String? device]) {
    var result = '$from says $msg';
    if (device != null) result += ' via $device';
    return result;
  }
  ```

### **2.2 命名参数 (Named Parameters)**

使用 `{}` 包裹。调用时必须指定参数名，顺序不限，更具可读性。

- **默认可选**：可以为 `null`。

- **必传标记**：使用 `required` 关键字。

  Dart

  ```dart
  void enableFlags({bool? bold, required bool hidden}) { ... }
  
  // 调用
  enableFlags(hidden: true, bold: false);
  ```

### **2.3 参数默认值**

可以使用 `=` 为可选参数设置默认值（必须是编译时常量）。

Dart

```dart
void setSettings({int volume = 50}) { ... }
```

------

## 3. 匿名函数与闭包

### **3.1 匿名函数 (Anonymous Functions)**

没有名字的函数，常用于集合操作。

Dart

```dart
var list = ['apples', 'bananas', 'oranges'];
list.forEach((item) {
  print('${list.indexOf(item)}: $item');
});
```

### **3.2 闭包 (Closures)**

闭包是一个函数对象，它可以访问其定义时所在作用域内的变量，即使该函数在外部被调用。

Dart

```dart
Function makeAdder(int addBy) {
  return (int i) => addBy + i; // 捕获了变量 addBy
}

void main() {
  var add2 = makeAdder(2);
  print(add2(3)); // 输出 5
}
```

------

## 4. 异步函数 (Async Functions)

Dart 使用 `Future` 和 `Stream` 处理异步操作，配合 `async` 和 `await` 关键字使用。

- **Future**：表示一个延迟计算的结果。
- **async**：标记函数为异步，返回值会自动包装在 Future 中。
- **await**：等待异步任务完成（只能在 async 函数中使用）。

Dart

```dart
Future<String> lookUpVersion() async {
  var version = await fetchVersionFromServer(); 
  return version;
}
```

------

## 5. 高阶函数 (Higher-Order Functions)

函数可以作为另一个函数的参数。

Dart

```dart
void printElement(int element) => print(element);

var list = [1, 2, 3];
list.forEach(printElement); // 将函数作为参数传递
```

------

## 6. 函数相关特性总结

| **特性**       | **描述**                                                 |
| -------------- | -------------------------------------------------------- |
| **一等公民**   | 函数可以赋值给变量，如 `var hi = (name) => 'Hi $name';`  |
| **词法作用域** | 变量的作用域由代码的布局决定，内部函数可以访问外部变量。 |
| **返回值**     | 所有函数都有返回值。如果没有指定，则隐式返回 `null`。    |
| **typedef**    | 用于为函数类型定义简短的别名，便于重用。                 |

------

### **示例：使用 typedef 定义函数签名**

Dart

```Dart
typedef IntListFilter = bool Function(int);

void filterList(List<int> list, IntListFilter predicate) {
  // ... 逻辑
}
```