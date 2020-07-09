# Anroid：JNI和NDK

### 目录![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS02MGZhNmQ3MDQwOGQxMzg4LnBuZz9pbWFnZU1vZ3IyL2F1dG8tb3JpZW50L3N0cmlwJTdDaW1hZ2VWaWV3Mi8yL3cvMTI0MA)

***

### 1. JNI介绍

#### 1.1 简介

- 定义：`Java Native Interface`，即 `Java`本地接口；
- 作用： 使得`Java` 与 本地其他类型语言（如`C、C++`）交互
- 特别注意：
  - `JNI`是 `Java` 调用 `Native` 语言的一种特性
  - `JNI` 是属于 `Java` 的，与 `Android` 无直接关系

---

#### 1.2 为什么要有JNI

- 背景：实际使用中，`Java` 需要与 本地代码 进行交互
- 问题：因为 `Java` 具备**跨平台**的特点，所以`Java` 与 本地代码交互的能力非常弱
- 解决方案： 采用 `JNI`特性 增强 `Java` 与 本地代码交互的能力

#### 1.3 实现步骤

1. 在`Java`中声明`Native`方法（即需要调用的本地方法）
2. 编译上述 `Java`源文件javac（得到 `.class`文件）
3. 通过 `javah` 命令导出`JNI`的头文件（`.h`文件）
4. 使用 `Java`需要交互的本地代码 实现在 `Java`中声明的`Native`方法

```
如 Java 需要与 C++ 交互，那么就用C++实现 Java的Native方法
```

5. 编译`.so`库文件
6. 通过`Java`命令执行 `Java`程序，最终实现`Java`调用本地代码