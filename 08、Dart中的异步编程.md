在 Dart 中，异步编程是处理耗时操作（如网络请求、文件 I/O、数据库操作）的核心技术。Dart 的异步模型基于 **单线程** 和 **事件循环（Event Loop）**。

------

## 1. 核心模型：事件循环 (Event Loop)

虽然 Dart 是单线程的，但它通过事件循环来处理异步任务。事件循环包含两个队列：

1. **微任务队列 (Microtask Queue)**：优先级最高，通常用于非常简短且需要立即执行的操作。
2. **事件队列 (Event Queue)**：用于处理外部事件（如 I/O、点击、定时器以及 Future）。

------

## 2. Future：单次异步结果

`Future<T>` 表示一个在未来某个时间点会完成的操作。它有三种状态：

- **Uncompleted (未完成)**：异步操作正在执行。
- **Completed with value (成功完成)**：返回了预期的结果。
- **Completed with error (异常完成)**：操作失败，抛出错误。

### **2.1 使用 `then` 处理结果**

Dart

```dart
void main() {
  fetchData().then((value) {
    print("获取到数据: $value");
  }).catchError((error) {
    print("发生错误: $error");
  }).whenComplete(() => print("操作结束"));
}
```

### **2.2 async 和 await (推荐)**

这是目前最常用的编写异步代码的方式，使异步逻辑看起来像同步代码一样清晰。

Dart

```dart
Future<void> displayData() async {
  try {
    // await 会挂起当前函数的执行，直到 Future 完成
    String data = await fetchData(); 
    print(data);
  } catch (e) {
    print("错误处理: $e");
  }
}
```

------

## 3. Stream：连续的异步事件

`Stream<T>` 就像一个异步的 `Iterable`（迭代器）。它不是返回单个值，而是随着时间的推移发出多个值。

### **3.1 监听 Stream**

- **单订阅 Stream**：只能有一个监听器（常用于文件下载、网络请求）。
- **广播 Stream (Broadcast)**：可以有多个监听器（常用于 UI 事件、传感器数据）。

Dart

```dart
// 使用 listen 监听
stream.listen((data) => print("接收到数据: $data"));

// 在异步函数中使用 await for 循环遍历
await for (var value in myStream) {
  print(value);
}
```

------

## 4. 生成器函数 (Generators)

Dart 提供了专门的语法来生成序列：

- **同步生成器 (`Iterable`)**：使用 `sync*` 和 `yield`。
- **异步生成器 (`Stream`)**：使用 `async*` 和 `yield`。

Dart

```dart
Stream<int> countStream(int max) async* {
  for (int i = 1; i <= max; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i; // 每次“产出”一个值到 Stream 中
  }
}
```

------

## 5. 并发处理：Isolate

由于 Dart 是单线程的，如果你执行复杂的 CPU 密集型计算（如大图片处理），会阻塞 UI 线程。此时需要使用 **Isolate**。

- **特点**：Isolate 之间不共享内存，每个 Isolate 都有自己的堆内存和事件循环。
- **通信**：通过 `SendPort` 和 `ReceivePort` 传递消息。

Dart

```dart
import 'dart:isolate';

void start() async {
  // 创建一个新的 Isolate 执行计算任务
  ReceivePort receivePort = ReceivePort();
  await Isolate.spawn(complexTask, receivePort.sendPort);
}

void complexTask(SendPort sendPort) {
  // 执行耗时 CPU 计算...
}
```

------

## 6. 常用异步操作符与工具

| **工具**                | **描述**                                     |
| ----------------------- | -------------------------------------------- |
| **`Future.wait([])`**   | 同时等待多个 Future 完成，返回结果列表。     |
| **`Future.delayed()`**  | 创建一个延迟执行的 Future（类似定时器）。    |
| **`StreamController`**  | 手动控制 Stream 的发射源。                   |
| **`StreamTransformer`** | 对 Stream 的数据进行转换（如格式化、过滤）。 |

------

## 7. 总结：如何选择？

1. **返回单个值**：使用 `Future`。
2. **返回一系列值**：使用 `Stream`。
3. **需要等待某个操作**：使用 `await`。
4. **不阻塞 UI 的 CPU 耗时计算**：使用 `Isolate`。