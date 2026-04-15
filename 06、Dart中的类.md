Dart 是一种面向对象的语言，支持基于类（Class-based）的继承和混合（Mixin）。在 Dart 中，所有的对象都是某个类的实例，而所有的类（除了 `Null`）都继承自 `Object`。

------

## 1. 类的声明与构造函数

### **1.1 基本声明**

使用 `class` 关键字声明类，使用 `new` 关键字（在 Dart 2.x 后可选）实例化。

Dart

```dart
class Point {
  double x = 0; // 实例变量
  double y = 0;

  // 默认构造函数
  Point(double x, double y) {
    this.x = x;
    this.y = y;
  }
}
```

### **1.2 语法糖：初始化形式参数**

Dart 提供了一种简写方式直接为成员变量赋值：

Dart

```dart
class Point {
  double x, y;
  Point(this.x, this.y); // 自动将参数赋值给同名成员变量
}
```

### **1.3 命名构造函数 (Named Constructors)**

Dart 不支持函数重载，因此使用命名构造函数来为类实现多个构造函数：

Dart

```dart
class Point {
  double x, y;
  Point(this.x, this.y);

  // 命名构造函数
  Point.origin() : x = 0, y = 0;
}
```

### **1.4 常量构造函数 (Constant Constructors)**

如果类生成的对象永远不会改变，可以将其定义为编译时常量。

- 要求：所有实例变量必须是 `final`。

Dart

```dart
class ImmutablePoint {
  final double x, y;
  const ImmutablePoint(this.x, this.y);
}
```

------

## 2. 实例变量、方法与 Getter/Setter

### **2.1 实例变量**

- 未初始化的实例变量值为 `null`。
- 所有变量隐式具有 Getter 方法，非 final 变量还有 Setter 方法。

### **2.2 Getter 和 Setter**

用于对属性访问进行拦截和处理：

Dart

```dart
class Rectangle {
  double left, top, width, height;

  Rectangle(this.left, this.top, this.width, this.height);

  // 使用 get 定义计算属性
  double get right => left + width;
  set right(double value) => left = value - width;
}
```

------

## 3. 继承与抽象类

### **3.1 继承 (Inheritance)**

使用 `extends` 关键字，子类可以使用 `super` 访问父类。

Dart

```dart
class Television {
  void turnOn() { ... }
}

class SmartTelevision extends Television {
  @override
  void turnOn() {
    super.turnOn();
    _connectToWifi();
  }
}
```

### **3.2 抽象类 (Abstract Classes)**

使用 `abstract` 修饰，不能被实例化，通常用于定义接口协议。

Dart

```dart
abstract class Doer {
  void doSomething(); // 抽象方法，没有方法体
}
```

------

## 4. 隐式接口 (Implicit Interfaces)

Dart 中没有 `interface` 关键字。**每个类都隐式定义了一个接口**，该接口包含该类所有的实例成员。

- 一个类可以通过 `implements` 实现一个或多个接口。

Dart

```dart
class Person {
  final String _name;
  Person(this._name);
  String greet(String who) => 'Hello, $who. I am $_name.';
}

class Impostor implements Person {
  @override
  String get _name => ''; 
  @override
  String greet(String who) => 'Hi $who. Do you know who I am?';
}
```

------

## 5. 混合 (Mixins)

Mixin 是一种在多个类层次结构中复用代码的方式（类似于“插件”）。使用 `with` 关键字。

Dart

```dart
mixin Logger {
  void log(String msg) => print('LOG: $msg');
}

class Service with Logger {
  void doWork() {
    log('Doing work...');
  }
}
```

------

## 6. 静态成员 (Static)

使用 `static` 关键字实现类级别的变量和方法，它们不属于特定的实例。

Dart

```dart
class Queue {
  static const initialCapacity = 16; // 静态常量
  static void printCapacity() {      // 静态方法
    print(initialCapacity);
  }
}
```

------

## 7. 扩展方法 (Extension Methods) - Dart 2.7+

可以在不修改原始类的情况下为其增加功能。

Dart

```dart
extension NumberParsing on String {
  int parseInt() {
    return int.parse(this);
  }
}

// 调用
print('42'.parseInt());
```

------

## 总结：类关系对比

| **特性** | **关键字**   | **描述**                         |
| -------- | ------------ | -------------------------------- |
| **继承** | `extends`    | 单继承，获取父类的实现。         |
| **实现** | `implements` | 实现接口，必须重写所有成员。     |
| **混入** | `with`       | 复用代码，非继承关系的逻辑共享。 |
| **抽象** | `abstract`   | 定义规范，强制子类实现。         |