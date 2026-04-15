在 Dart 中，泛型（Generics）是实现代码复用、减少重复代码并提供**类型安全**的重要工具。通过使用泛型，你可以编写可以应用于多种类型的类或方法，同时让编译器在编译阶段捕捉到类型不匹配的错误。

------

## 1. 为什么使用泛型？

- **类型安全**：在编译时检查类型，避免 `Runtime Error`（运行时错误）。
- **减少代码重复**：不需要为每种数据类型编写逻辑相同的类或方法。

> **示例：**
>
> 如果没有泛型，你可能需要为存储字符串和存储整数分别创建 `StringList` 和 `IntList`。有了泛型，只需一个 `List<T>`。

------

## 2. 泛型集合

这是泛型最常见的用法。Dart 中的 `List`、`Set` 和 `Map` 都是参数化类型。

Dart

```dart
// 限制 List 只能包含 String
var names = <String>[];
names.addAll(['Seth', 'Kathy', 'Lars']);
// names.add(42); // 编译错误：Argument type 'int' can't be assigned to 'String'

// Map 的泛型：<KeyType, ValueType>
var pages = <String, String>{
  'index.html': 'Homepage',
  'robots.txt': 'Hints for robots',
};
```

------

## 3. 泛型类 (Generic Classes)

你可以使用类型参数（通常用 `T`, `E`, `K`, `V` 等大写字母表示）定义自己的类。

Dart

```dart
class Cache<T> {
  T? _data;

  void setItem(T value) {
    _data = value;
  }

  T? getItem() => _data;
}

void main() {
  var stringCache = Cache<String>();
  stringCache.setItem("Hello Dart");
  
  var intCache = Cache<int>();
  intCache.setItem(123);
}
```

------

## 4. 限制类型范围 (Generic Constraints)

有时你希望泛型只能是某些特定类型。使用 `extends` 关键字可以限制参数类型的上限。

Dart

```dart
class SomeBaseClass {}
class Extender extends SomeBaseClass {}

// T 必须是 SomeBaseClass 或其子类
class Foo<T extends SomeBaseClass> {
  String toString() => "Instance of Foo<$T>";
}

var foo = Foo<Extender>(); // 正确
// var error = Foo<String>(); // 错误：String 不是 SomeBaseClass 的子类
```

------

## 5. 泛型方法 (Generic Methods)

方法也可以拥有自己的类型参数，独立于类。

Dart

```dart
T firstElement<T>(List<T> ts) {
  // 做一些初始化或处理...
  T tmp = ts[0];
  return tmp;
}

void main() {
  var result = firstElement<int>([1, 2, 3]);
  print(result); // 输出 1
}
```

------

## 6. 运行时中的泛型 (Generics are Reified)

与 Java（使用类型擦除）不同，Dart 的泛型在运行时是**具体化（Reified）**的。这意味着在运行时，对象仍然携带其泛型类型信息。

Dart

```dart
var names = <String>[];
print(names is List<String>); // 输出 true
print(names.runtimeType);      // 输出 List<String>
```

------

## 7. 常用类型参数命名习惯

虽然你可以使用任何标识符，但社区习惯使用单字母：

- `E`: 集合元素 (Element)
- `K`: Map 的键 (Key)
- `V`: Map 的值 (Value)
- `T`: 通用类型 (Type)
- `R`: 返回值类型 (Return type)
- `S`, `U`: 更多类型参数

------

## 总结

| **特性**                 | **描述**                                           |
| ------------------------ | -------------------------------------------------- |
| **具体化 (Reified)**     | 运行时保留泛型信息，支持 `is` 检查。               |
| **类型约束 (`extends`)** | 限制泛型只能是特定类或其派生类。                   |
| **类型安全**             | 在开发阶段通过编辑器反馈和编译器检查防止类型错误。 |
| **集合支持**             | `List<T>`, `Set<T>`, `Map<K, V>` 深度集成泛型。    |