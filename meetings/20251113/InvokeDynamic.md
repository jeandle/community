# `invokedynamic` 指令详解

`invokedynamic` 是 Java 虚拟机（JVM）在 **Java 7（[JSR 292](https://jcp.org/en/jsr/detail?id=292)）** 中引入的一条全新字节码指令，其核心目标是支持**动态类型语言**在 JVM 上高效运行，并为 Java 语言自身的演进（如 Lambda 表达式、字符串拼接 等）提供底层基础设施。它通过将**方法链接（linkage）决策延迟到运行时**，突破了传统 JVM 调用指令对静态类型的依赖。

## 一、由来：为何需要 `invokedynamic`？

### 1.1 传统 JVM 调用指令的局限

在 Java 7 之前，JVM 提供了四种方法调用指令：

| 指令 | 绑定时机 | 适用场景 |
|------|--------|--------|
| `invokestatic` | 编译期（静态） | 静态方法 |
| `invokespecial` | 编译期（静态） | 构造器、私有方法、父类方法 |
| `invokevirtual` | 运行时（动态,基于类继承） | 虚方法（普通实例方法） |
| `invokeinterface` | 运行时（动态,基于接口） | 接口方法 |

这些指令都依赖 **编译期已知的方法签名和接收者类型**，适用于**静态类型语言**（如 Java），但无法高效表达**动态类型语言**（如 Ruby, Python, JavaScript）的核心特性：

> 方法名、接收者类型、甚至方法是否存在，都只能在运行时确定。

例如，在 Ruby 中：

```
# Greeter.rb
class Greeter
  def hello
    "Hello!"
  end
end

g = Greeter.new
method_name = "hello"
result = g.send(method_name) # 动态调用 Greeter#hello
puts result                  # 输出 "Hello!"
```

传统字节码无法直接表达此类动态调用语义。

### 1.2 反射性能低下

早期动态语言在JVM上的实现（如 JRuby 1.6）依赖 Java 反射模拟动态调用：

```
Class<?> clazz = g.getClass();
Method method = clazz.getMethod(methodName);
String result = (String)method.invoke(g);  # 调用 Greeter.hello
System.out.println(result);                # 输出 "Hello!"
```

由于反射每次调用都需安全检查、参数装箱/拆箱，且 JIT 难以优化，反射无法满足高频动态调用的性能需求。

### 1.3 MethodHandle：更高效的替代方案

Java 7 引入了 java.lang.invoke.MethodHandle，提供接近直接调用的性能：

```
MethodHandles.Lookup lookup = MethodHandles.lookup();
MethodType mt = MethodType.methodType(void.class);
MethodHandle mh = getMethodHandle(lookup, methodName, mt);
String result = (String)mh.invoke(g);  # 调用 Greeter.hello
System.out.println(result);            # 输出 "Hello!"

/**
 * 此方法可以根据需求定制化实现，只要最后返回符合签名要求的MethodHandle即可
 * @param lookup 用于查找方法的上下文（含访问权限）
 * @param name   目标方法名（如 "hello"）
 * @param mt     期望的方法签名（参数+返回类型）
 * @param args   可选静态参数（来自 invokedynamic 的附加常量）
 * @return 符合签名的 MethodHandle
 */
public static MethodHandle getMethodHandle(Lookup lookup, String name, MethodType mt, Object... args) {
    try {
        // 可以按规则查找特定的方法
        return lookup.findStatic(Greeter.class, name, mt);
    } catch (NoSuchMethodException | IllegalAccessException e) {
        // 也可以现场构造方法
        return MethodHandles.constant(String.class, "Hello!");
    }
}
```
说明：上述伪代码代码基本模拟了 `invokedynamic` 的核心思想 —— 运行时获取MethodHandle，通过用户自定义的getMethodHandle，将方法链接逻辑交给用户自定义，而非由 JVM 在编译期硬编码。

## 二、`invokedynamic` 在 JVM 中的实现机制

### 2.1 `invokedynamic`字节码解析

一个典型的 `invokedynamic` 和 `invokestatic` 调用在字节码中如下所示：

```
ConstantPool:
   #7 = InvokeDynamic      #0:#8          // #0:run:()Ljava/lang/Runnable;
   #8 = NameAndType        #9:#10         // run:()Ljava/lang/Runnable;
   #9 = Utf8               run
  #10 = Utf8               ()Ljava/lang/Runnable;
  #12 = Class              #14            // Invoke
  #24 = Methodref          #12.#25        // Invoke.inDy:(Ljava/lang/String;Ljava/lang/String;)Ljava/lang/String;
  #25 = NameAndType        #26:#27        // inDy:(Ljava/lang/String;Ljava/lang/String;)Ljava/lang/String;
  #75 = MethodType         #6             //  ()V
  #76 = MethodHandle       6:#77          // REF_invokeStatic Invoke.lambda$inDy$0:()V
  #77 = Methodref          #12.#78        // Invoke.lambda$inDy$0:()V
  #78 = NameAndType        #70:#6         // lambda$inDy$0:()V
  #79 = MethodHandle       6:#80          // REF_invokeStatic java/lang/invoke/LambdaMetafactory.metafactory:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite;

Bytecode invokedynamic:
    0: invokedynamic #7,  0              // InvokeDynamic #0:run:()Ljava/lang/Runnable;
    5: putstatic     #11                 // Field holder:Ljava/lang/Runnable;

Bytecode invokestatic:
    6: invokestatic  #24                 // Method inDy:(Ljava/lang/String;Ljava/lang/String;)Ljava/lang/String;
    9: putstatic     #28                 // Field helloWorldStr:Ljava/lang/String;

BootstrapMethods:
  0: #79 REF_invokeStatic java/lang/invoke/LambdaMetafactory.metafactory:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite;
    Method arguments:
      #75 ()V
      #76 REF_invokeStatic Invoke.lambda$inDy$0:()V
      #75 ()V
```
invokedynamic`字节码的组成： 
- `invokedynamic`指令 -> 单字节 java字节码标识符
- `InvokeDynamic`常量 -> 指明字节码在常量池中的信息
- `BootstrapMethods`属性 -> 用于获取目标MethodHandle

>  **为什么BootstrapMethods返回的是CallSite?**

### 2.2 核心组件

`invokedynamic` 依赖三个核心组件协同工作：

| 组件 | 说明 |
|------|------|
| **引导方法（Bootstrap Method, BSM）** | 一个Java 方法，在首次调用时由 JVM 执行，负责解析并返回 `CallSite`。 |
| **调用点（CallSite）** | 封装了目标方法的 `MethodHandle`，后续调用直接通过它执行。 |
| **方法句柄（MethodHandle）** | 强类型的、可直接调用的方法引用，是 `invokedynamic` 的底层执行单元。 |

>  **为什么把MethodHandle封装为CallSite?**

### 2.3 调用流程

1. **首次执行**：
   - JVM 查找并调用 BSM；
   - BSM 根据方法名、参数类型等信息动态解析目标方法；
   - 返回一个 `CallSite`（通常为 `ConstantCallSite` 或 `MutableCallSite`）。
2. **后续调用**：直接调用 `CallSite.target.invoke(...)`，JVM 可对其进行内联、去虚拟化等优化。

>  **为什么BSM只需调用一次?**

### 2.4 `invokedynamic`的链接机制

`invokedynamic` 的链接分为两层，兼顾性能与动态性，主要为来解决以下两个问题：
- BSM只调用一次并返回，如何做到运行时修改MethodHandle？
- MethodHandle是java方法引用，包含分派语义，如何实现？（MethodHandle的实现机制）


####  2.4.1 CallSite ---- 运行时可变目标的载体
`CallSite`可以大致分为`ConstantCallSite`和`MutableCallsite`，分别对应MethodHandle在运行时是否可变。
用例如下：
```
// HelloInvoker.jasm
super public class HelloInvoker
    version 52:0
{
    public static Method doit:"()V"
    stack 3 locals 1
    {
     ldc           String "hello!";
     invokedynamic InvokeDynamic REF_invokeStatic:HelloInvoke.myBSM:"(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite;":callme:"(Ljava/lang/String;)V";
     return;
    }
}
```
```
// HelloInvoker.class
Constant pool:
   #2 = InvokeDynamic      #0:#11         // #0:callme:(Ljava/lang/String;)V
  #11 = NameAndType        #5:#4          // callme:(Ljava/lang/String;)V
{
  public static void doit();
    descriptor: ()V
    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
    Code:
      stack=3, locals=1, args_size=0
         0: ldc           #1                  // String hello!
         2: invokedynamic #2,  0              // InvokeDynamic #0:callme:(Ljava/lang/String;)V
         7: return
}
SourceFile: "HelloInvoker.jasm"
BootstrapMethods:
  0: #18 REF_invokeStatic HelloInvoke.myBSM:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite;
    Method arguments:
```
```
// HelloInvoke.java
import java.lang.invoke.*;
public class HelloInvoke {
    public static MutableCallSite dynamicCallsite; // 全局可变调用点，用于后续修改目标方法
    public static String str;
    public static void main(String args[]) throws Throwable {
        for (int i = 0; i < 200; i++) {
            HelloInvoker.doit();  // 调用会执行 callme("hello!")
        }
        System.out.println("[before setTarget]HelloInvoke.str = " + str);
        MethodHandles.Lookup lookup = MethodHandles.lookup();
        MethodType mt = MethodType.methodType(void.class, String.class);
        MethodHandle newTarget = lookup.findStatic(HelloInvoke.class, "callnone", mt);
        dynamicCallsite.setTarget(newTarget); // 动态替换目标方法为 callnone
        HelloInvoker.doit();  // 现在会执行 callnone("hello!")
        System.out.println("[after  setTarget]HelloInvoke.str = " + str);
    }
 
    static CallSite myBSM(MethodHandles.Lookup lookup,
           String name, MethodType type) throws Throwable {
        MethodType mt = MethodType.methodType(void.class, String.class);
        MethodHandle mh = lookup.findStatic(HelloInvoke.class, name, mt); // 根据名称和签名查找目标方法
        // return new ConstantCallSite(mh.asType(type));
        dynamicCallsite = new MutableCallSite(mh.asType(type)); // 创建可变调用点（MutableCallSite），允许后续 setTarget()
        return dynamicCallsite;
    }
 
    static void callme(String x) {
        str = x;
    }

    static void callnone(String x) {
        str = "callnone!";
    }
}
```
输出结果如下：
说明 MutableCallSite 允许在运行时动态改变方法调用目标。
```
[before setTarget]HelloInvoke.str = hello!
[after  setTarget]HelloInvoke.str = callnone!
```

上述返回ConstantCallSite和返回MutableCallSite的执行栈帧如下 (-XX:+ShowHiddenFrames)：
```
ConstantCallSite：                              
        at HelloInvoke.callme(HelloInvoke.java:28)
        at java.base/java.lang.invoke.LambdaForm$DMH000/0x00007afec05030c0.invokeStatic000_LL_V(LambdaForm$DMH000)
        at java.base/java.lang.invoke.LambdaForm$MH001/0x00007afec0504070.linkToTargetMethod000_LL_V(LambdaForm$MH001)
        at HelloInvoker.doit(HelloInvoker.jasm)
        at HelloInvoke.main(HelloInvoke.java:7)
MutableCallsite：
        at HelloInvoke.callnone(HelloInvoke.java:31)
        at java.base/java.lang.invoke.LambdaForm$DMH000/0x00007ec3985030c0.invokeStatic000_LL_V(LambdaForm$DMH000)
        at java.base/java.lang.invoke.LambdaForm$MH001/0x00007ec3985040d0.linkToCallSite000_LL_V(LambdaForm$MH001)
        at HelloInvoker.doit(HelloInvoker.jasm)
        at HelloInvoke.main(HelloInvoke.java:14)
```
- ConstantCallSite
```
   @Hidden
   @Compiled
   @ForceInline
   static void linkToTargetMethod000_LL_V(Object var0, Object var1) { // var1: appendix_oop
      ((MethodHandle)var1).invokeBasic(var0);
   }
```
- MutableCallsite
```
   @Hidden
   @Compiled
   @ForceInline
   static void linkToCallSite000_LL_V(Object var0, Object var1) { // var1: appendix_oop
      MethodHandle var2 = Invokers.getCallSiteTarget((CallSite)var1);
      ((MethodHandle)var2).invokeBasic(var0);
   }
```
- `invokedynamic` 把 BSM 返回的 `MethodHandle`/`CallSite` 绑定给方法调用点，并将其作为后续调用的入参。
- `MethodHandle` 封装成 `CallSite` 可以屏蔽调用点对 `MethodHandle` 变化的感知，使得 `invokedynamic` 在调用处理上和 `invokestatic` 一致，无需多次调用BSM.

####  2.4.2 MethodHandle实现java方法的分派语义

- ConstantCallSite
```
   @Hidden
   @Compiled
   @ForceInline
   static void invokeStatic000_LL_V(Object var0, Object var1) {
      Object var2 = DirectMethodHandle.internalMemberName(var0);
      MethodHandle.linkToStatic(var1, (MemberName)var2);
   }
```
- MutableCallsite
```
   @Hidden
   @Compiled
   @ForceInline
   static void invokeStatic000_LL_V(Object var0, Object var1) {
      Object var2 = DirectMethodHandle.internalMemberName(var0);
      MethodHandle.linkToStatic(var1, (MemberName)var2);
   }
```
MethodHandle.linkToStatic是JVM内部的intrinsic，解释为伪代码的话大概如下：
```
    // memberName 包含目标方法的元信息（类、方法名、签名等）
    Method* method = memberName.method.vmtarget;

    if (method == null) throw new AbstractMethodError();

    if (method.isCompiled()) {
      jmp method->from_compiled_entry();
    } else {
      jmp method->interpret_entry();
    }
```

除了linkToStatic，JVM还有linkToSpecial, linkToVirtual, linkToInterface等，分别对应之前的调用类型。

通过这两层的链接，使得 `invokedynamic` 在搭配一个足够灵活的 `BSM` 之后，在满足**方法签名**和**访问约束**的前提下，几乎可以在运行时随意修改调用目标，达到调用点和调用目标解耦的效果。

## 三、`invokedynamic` 的实际应用与意义

- 动态语言支持：JRuby, Nashorn（JS）, Groovy 等借助 `invokedynamic` 实现高性能运行；
- Java 语言特性：
  - Lambda 表达式：通过 `invokedynamic` 在运行时动态生成适配器。其引导方法为 java.lang.invoke.LambdaMetafactory.metafactory。
    ```
    // Invoke.java
    public static Runnable holder;
    public static void inDy(Invoke invoke) {
        // holder = new Runnable() {  // Invoke$1
        //     @Override
        //     public void run() {
        //         System.out.println(invoke.str);
        //     }
        // };
        holder = () -> System.out.println(invoke.str); // Invoke$$Lambda
    }
    ```
    ```
    // Invoke.class
    public static void inDy(Invoke);
    descriptor: (LInvoke;)V
    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
    Code:
      stack=1, locals=1, args_size=1
        0: aload_0
        1: invokedynamic #7,  0              // InvokeDynamic #0:run:(LInvoke;)Ljava/lang/Runnable;
        6: putstatic     #11                 // Field holder:Ljava/lang/Runnable;
        9: return

    private static void lambda$inDy$0(Invoke);
    descriptor: (LInvoke;)V
    flags: (0x100a) ACC_PRIVATE, ACC_STATIC, ACC_SYNTHETIC
    Code:
      stack=2, locals=1, args_size=1
        0: getstatic     #50                 // Field java/lang/System.out:Ljava/io/PrintStream;
        3: aload_0
        4: getfield      #39                 // Field str:Ljava/lang/String;
        7: invokevirtual #56                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        10: return
      LineNumberTable:
        line 9: 0

    BootstrapMethods:
      0: #89 REF_invokeStatic java/lang/invoke/LambdaMetafactory.metafactory:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite;
        Method arguments:
          #85 ()V
          #86 REF_invokeStatic Invoke.lambda$inDy$0:(LInvoke;)V
          #85 ()V
    ```
    ```
    // Invoke$$Lambda.0x00007ffdc4700250.java
    final class Invoke$$Lambda implements Runnable {
      private final Invoke arg$1;
      private Invoke$$Lambda(Invoke var1) {
          this.arg$1 = var1;
      }
      public void run() {
          Invoke.lambda$inDy$0(this.arg$1);
      }
    }
    ```
    优势：相比匿名内部类（编译期生成 Invoke$1.class），Lambda 通过 invokedynamic 延迟到运行时按需生成实现类，避免了类文件膨胀，且 JVM 可对 CallSite 进行内联优化，性能更高。

  - 字符串拼接（+）在 JDK 9+ 默认使用 `invokedynamic` 优化；
    ```
    // Invoke.java
    public static String inDy(String a, String b) {
        return a + ", " + b + "!";
    }
    public static void doInvoke() {
        System.out.println(inDy("Hello", "world"));
    }
    ```
    ```
    public static java.lang.String inDy(java.lang.String, java.lang.String);
      descriptor: (Ljava/lang/String;Ljava/lang/String;)Ljava/lang/String;
      flags: (0x0009) ACC_PUBLIC, ACC_STATIC
      Code:
        stack=2, locals=2, args_size=2
          0: aload_0
          1: aload_1
          2: invokedynamic #7,  0              // InvokeDynamic #0:makeConcatWithConstants:(Ljava/lang/String;Ljava/lang/String;)Ljava/lang/String;
          7: areturn
        LineNumberTable:
          line 18: 0

    SourceFile: "Invoke.java"
    BootstrapMethods:
      0: #79 REF_invokeStatic java/lang/invoke/StringConcatFactory.makeConcatWithConstants:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;
        Method arguments:
          #77 \u0001, \u0001!
    ```
    效果：字节码体积减小、避免重复创建 StringBuilder、支持运行时根据参数类型（常量/变量）和数量动态优化，显著提升高频拼接性能

- 提供低开销的动态语言能力；

总结: `invokedynamic` 并未改变 JVM 的静态类型本质，而是为其注入了可控的动态性。它是 JVM 演进史上一次精巧的“兼容式创新”——在不破坏现有生态的前提下，为未来打开了更多可能。
