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

**更加详细过程请参考本文第4节：具体使用**

### 2. NDK介绍

#### 2.1 简介

- 定义：`Native Development Kit`，是 `Android`的一个工具开发包

```
NDK是属于 Android 的，与Java并无直接关系
```

- 作用：快速开发`C`、 `C++`的动态库，并自动将`so`和应用一起打包成 `APK`

```
即可通过 NDK在 Android中 使用 JNI与本地代码（如C、C++）交互
```

- 应用场景：在**Android的场景下** 使用JNI

```
即 Android开发的功能需要本地代码（C/C++）实现
```

- 特点

![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS1mNjYzNDJkZjU2OGVmN2FhLnBuZz9pbWFnZU1vZ3IyL2F1dG8tb3JpZW50L3N0cmlwJTdDaW1hZ2VWaWV3Mi8yL3cvMTI0MA)

- 额外注意

![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS0yYzc3NzFiNzFjZTU1OWFhLnBuZz9pbWFnZU1vZ3IyL2F1dG8tb3JpZW50L3N0cmlwJTdDaW1hZ2VWaWV3Mi8yL3cvMTI0MA)

#### 2.2  使用步骤

1. 配置 `Android NDK`环境
2. 创建 `Android` 项目，并与 `NDK`进行关联
3. 在 `Android` 项目中声明所需要调用的 `Native`方法
4. 使用 `Android`需要交互的本地代码 实现在`Android`中声明的`Native`方法

```
比如 Android 需要与 C++ 交互，那么就用C++ 实现 Java的Native方法
```

5. 通过 `ndk - bulid` 命令编译产生`.so`库文件
6. 编译 `Android Studio` 工程，从而实现 `Android` 调用本地代码

**更加详细过程请参考本文第4节：具体使用**

### 3. NDK与JNI关系

![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS02NjA3ZTkzMjFkM2NiZGRkLnBuZz9pbWFnZU1vZ3IyL2F1dG8tb3JpZW50L3N0cmlwJTdDaW1hZ2VWaWV3Mi8yL3cvMTI0MA)

---

### 4. 具体使用

本文根据版本的不同介绍了两种在`Android Studio`中实现 `NDK`的方法：`Android Studio`2.2 以下 & 2.2以上

#### 4.1 `Android Studio`2.2 以下实现NDK

- 步骤如下

  1. 配置 `Android NDK`环境
  2. 关联 `Andorid Studio`项目 与 `NDK`
  3. 创建本地代码文件（即需要在 `Android`项目中调用的本地代码文件）
  4. 创建 `Android.mk`文件 & `Application.mk`文件
  5. 编译上述文件，生成`.so`库文件，并放入到工程文件中
  6. 在 `Andoird Studio`项目中使用 `NDK`实现 `JNI` 功能

- 步骤详解

  - **步骤1：配置 Android NDK环境**

    - 具体请看文章[手把手教你配置Android NDK环境](http://blog.csdn.net/carson_ho/article/details/73250111)

  - **步骤2： 关联Andorid Studio项目 与 NDK**

    -  当你的项目每次需要使用 `NDK` 时，都需要将该项目关联到 `NDK`

    ```
    1. 此处使用的是Andorid Studio，与Eclipse不同
    2. 还在使用Eclipse的同学请自行查找资料配置
    ```

    - 具体配置如下

      1. **在`Gradle`的 `local.properties`中添加配置**

      ```
      ndk.dir=/Users/Carson_Ho/Library/Android/sdk/ndk-bundle
      ```

      >若`ndk`目录存放在`SDK`的目录中，并命名为`ndk-bundle`，则该配置自动添加

      ![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS1jMTI5OWJhZjEzMDMzNTZkLnBuZz9pbWFnZU1vZ3IyL2F1dG8tb3JpZW50L3N0cmlwJTdDaW1hZ2VWaWV3Mi8yL3cvMTI0MA)

      2.  **在`Gradle`的 `gradle.properties`中添加配置**

      ```
      android.useDeprecatedNdk=true 
      // 对旧版本的NDK支持
      ```

      ![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS00YjczMTMwZTdlZTFmZmMxLnBuZz9pbWFnZU1vZ3IyL2F1dG8tb3JpZW50L3N0cmlwJTdDaW1hZ2VWaWV3Mi8yL3cvMTI0MA)

      - **至此，将Andorid Studio的项目 与 NDK 关联完毕**
      - 下面，将真正开始讲解如何在项目中使用NDK

  - 步骤3：创建本地代码文件
  
    - 即需要在Android项目中调用的本地代码文件
  
      > 此处采用 `C++`作为展示

```c++
# include <jni.h>
# include <stdio.h>

extern "C"
{
   
    JNIEXPORT jstring JNICALL Java_scut_carson_1ho_ndk_1demo_MainActivity_getFromJNI(JNIEnv *env, jobject obj ){
       // 参数说明
       // 1. JNIEnv：代表了VM里面的环境，本地的代码可以通过该参数与Java代码进行操作
       // 2. obj：定义JNI方法的类的一个本地引用（this）
    return env -> NewStringUTF("Hello i am from JNI!");
    // 上述代码是返回一个String类型的"Hello i am from JNI!"字符串
	}
}
```

此处需要注意：

- 如果本地代码是`C++`（`.cpp`或者`.cc`），要使用`extern "C" { }`把本地方法括进去

- `JNIEXPORT jstring JNICALL`中的`JNIEXPORT` 和 `JNICALL`不能省

- 关于方法名`Java_scut_carson_1ho_ndk_1demo_MainActivity_getFromJNI`

  1. 格式 = `Java _包名 _ 类名_Java需要调用的方法名`
  2. `Java`必须大写
  3. 对于包名，包名里的`.`要改成`_`，`_`要改成`_1`

  > 如我的包名是：`scut.carson_ho.ndk_demo`，则需要改成`scut_carson_1ho_ndk_1demo`

最后，将创建好的*test.cpp*文件放入到工程文件目录中的`src/main/jni`文件夹

	> 若无`jni`文件夹，则手动创建。

下面我讲解一下JNI类型与Java类型对应的关系介绍

![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS1lMmQzMjE4ZTJiZjA5MzRmLnBuZz9pbWFnZU1vZ3IyL2F1dG8tb3JpZW50L3N0cmlwJTdDaW1hZ2VWaWV3Mi8yL3cvMTI0MA)

- **步骤4：创建Android.mk文件**

  - 作用：指定源码编译的配置信息

  > 如工作目录，编译模块的名称，参与编译的文件等
  - 具体使用

  *[Android.mk](http://android.mk/)*

  ```c++
  LOCAL_PATH       :=  $(call my-dir)
  // 设置工作目录，而my-dir则会返回Android.mk文件所在的目录
  
  include              $(CLEAR_VARS)
  // 清除几乎所有以LOCAL——PATH开头的变量（不包括LOCAL_PATH）
  
  LOCAL_MODULE     :=  hello_jni
  // 设置模块的名称，即编译出来.so文件名
  // 注，要和上述步骤中build.gradle中NDK节点设置的名字相同
  
  LOCAL_SRC_FILES  :=  test.cpp
  // 指定参与模块编译的C/C++源文件名
  
  include              $(BUILD_SHARED_LIBRARY)
  // 指定生成的静态库或者共享库在运行时依赖的共享库模块列表。
  
  
  ```

  最后，将上述文件同样放在`src/main/jni`文件夹中。

- **步骤5: 创建Application.mk文件**

  - 作用：配置编译平台相关内容
  - 具体使用

  *[Application.mk](http://application.mk/)*

  ```
  APP_ABI := armeabi
  // 最常用的APP_ABI字段：指定需要基于哪些CPU平台的.so文件
  // 常见的平台有armeabi x86 mips，其中移动设备主要是armeabi平台
  // 默认情况下，Android平台会生成所有平台的.so文件，即同APP_ABI := armeabi x86 mips
  // 指定CPU平台类型后，就只会生成该平台的.so文件，即上述语句只会生成armeabi平台的.so文件
  ```

  最后，将上述文件同样放在`src/main/jni`文件夹中

- **步骤6:编译上述文件，生成.so库文件**

  - 经过上述步骤，在`src/main/jni`文件夹中已经有3个文件

  ![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS01MzM0NjFiNzQxYWY0NDdlLnBuZz9pbWFnZU1vZ3IyL2F1dG8tb3JpZW50L3N0cmlwJTdDaW1hZ2VWaWV3Mi8yL3cvMTI0MA)

  - 打开终端，输入以下命令

  ```
  // 步骤1：进入该文件夹
  cd /Users/Carson_Ho/AndroidStudioProjects/NDK_Demo/app/src/main/jni 
  // 步骤2：运行NDK编译命令
  ndk-build
  ```

  ![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS04Y2E2NDI4ODQyNjVlMjg0LnBuZz9pbWFnZU1vZ3IyL2F1dG8tb3JpZW50L3N0cmlwJTdDaW1hZ2VWaWV3Mi8yL3cvMTI0MA)

  - 编译成功后，在`src/main/`会多了两个文件夹`libs` & `obj`，其中`libs`下存放的是`.so`库文件

![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS1kODNlZDY1ZjFkNjM2ODMzLnBuZz9pbWFnZU1vZ3IyL2F1dG8tb3JpZW50L3N0cmlwJTdDaW1hZ2VWaWV3Mi8yL3cvMTI0MA)

- **步骤7:在`src/main/`中创建一个名为`jniLibs`的文件夹，并将上述生成的so文件夹放到该目录下**

  1. 要把名为 `CPU`平台的文件夹放进去，而不是把`.so`文件放进去
  2. 如果本来就有.so文件，那么就直接创建名为`jniLibs`的文件夹并放进去就可以

  ![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS02NjRlMTcwYzgzOGZlZWMxLnBuZz9pbWFnZU1vZ3IyL2F1dG8tb3JpZW50L3N0cmlwJTdDaW1hZ2VWaWV3Mi8yL3cvMTI0MA)

- **步骤8:在Andoird Studio项目中使用NDK实现JNI功能**

  - 此时，我们已经将本地代码文件编译成`.so`库文件并放入到工程文件中
  - 在`Java`代码中调用本地代码中的方法，具体代码如下：

  *MainActivity.java*

  ```java
  public class MainActivity extends AppCompatActivity  {
  
      // 步骤1:加载生成的so库文件
      // 注意要跟.so库文件名相同
      static {
  
          System.loadLibrary("hello_jni");
      }
      
      // 步骤2:定义在JNI中实现的方法
      public native String getFromJNI();
      
      // 此处设置了一个按钮用于触发JNI方法
      private Button Button;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
  
          // 通过Button调用JNI中的方法
          Button = (Button) findViewById(R.id.button);
          Button.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  Button.setText(getFromJNI());
                  
              }
          });
      }
  ```

  主布局文件：*activity_main.xml*

  ```xml
  <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:tools="http://schemas.android.com/tools"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:paddingBottom="@dimen/activity_vertical_margin"
      android:paddingLeft="@dimen/activity_horizontal_margin"
      android:paddingRight="@dimen/activity_horizontal_margin"
      android:paddingTop="@dimen/activity_vertical_margin"
      tools:context="scut.carson_ho.ndk_demo.MainActivity">
  
      // 此处设置了一个按钮用于触发JNI方法
      <Button
          android:id="@+id/button"
          android:layout_centerInParent="true"
          android:layout_width="300dp"
          android:layout_height="50dp"
          android:text="调用JNI代码" />
  
  </RelativeLayout>
  
  ```

  ### 结果展示

  ![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS0xZjYwODY0OTljYTJjNjljLmdpZj9pbWFnZU1vZ3IyL2F1dG8tb3JpZW50L3N0cmlw)

---

#### 4.2 `Android Studio`2.2 以上实现NDK

- 如果你的`Android Studio`是2.2以上的，那么请采用下述方法

> 因为`Android Studio`2.2以上已经内部集成 `NDK`，所以只需要在`Android Studio`内部进行配置就可以

- 步骤讲解

**步骤1:按提示创建工程**

在创建工程时，需要配置 `NDK`，根据提示一步步安装即可。

![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS1mMWM2ZjkyZmM1OWRjYmVjLnBuZz9pbWFnZU1vZ3IyL2F1dG8tb3JpZW50L3N0cmlwJTdDaW1hZ2VWaWV3Mi8yL3cvMTI0MA)

**步骤2:根据需求使用NDK**

- 配置好`NDK`后，`Android Studio`会自动生成`C++`文件并设置好调用的代码
- 你只需要根据需求修改`C++`文件 & `Android`就可以使用了

![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS0xZmRjYjFmNzdmMmFkZWZmLnBuZz9pbWFnZU1vZ3IyL2F1dG8tb3JpZW50L3N0cmlwJTdDaW1hZ2VWaWV3Mi8yL3cvMTI0MA)

### 5.  总结

- 本文主要讲解 `Java`的 `JNI`与 `Android`的`NDK`相关知识
- 下面我将继续对 `Android`中的`NDK`进行深入讲解 ，有兴趣可以继续关注[Carson_Ho的安卓开发笔记](https://blog.csdn.net/carson_ho)

