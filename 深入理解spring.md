# 深入理解spring

## 先决条件

### 反射

获得类的实例信息，然后就可以操作字段方法

```java
三种方式获取方式
Class cls = String.class;

String s = "Hello";
Class cls = s.getClass();

Class cls = Class.forName("java.lang.String");
```

- Field getField(name)：根据字段名获取某个public的field（包括父类）
- Field getDeclaredField(name)：根据字段名获取当前类的某个field（不包括父类）
- Field[] getFields()：获取所有public的field（包括父类）
- Field[] getDeclaredFields()：获取当前类的所有field（不包括父类）
- `Method getMethod(name, Class...)`：获取某个`public`的`Method`（包括父类）
- `Method getDeclaredMethod(name, Class...)`：获取当前类的某个`Method`（不包括父类）
- `Method[] getMethods()`：获取所有`public`的`Method`（包括父类）
- `Method[] getDeclaredMethods()`：获取当前类的所有`Method`（不包括父类）

### 注解

java中用于标识的特殊"注释"

第一类是由编译器使用的注解，例如：

- `@Override`：让编译器检查该方法是否正确地实现了覆写；
- `@SuppressWarnings`：告诉编译器忽略此处代码产生的警告。

这类注解不会被编译进入`.class`文件，它们在编译后就被编译器扔掉了。

第二类是由工具处理`.class`文件使用的注解，比如有些工具会在加载class的时候，对class做动态修改，实现一些特殊的功能。这类注解会被编译进入`.class`文件，但加载结束后并不会存在于内存中。这类注解只被一些底层库使用，一般我们不必自己处理。

第三类是在程序运行期能够读取的注解，它们在加载后一直存在于JVM中，这也是最常用的注解。例如，一个配置了`@PostConstruct`的方法会在调用构造方法后自动被调用（这是Java代码读取该注解实现的功能，JVM并不会识别该注解）。

#### @Target

最常用的元注解是`@Target`。使用`@Target`可以定义`Annotation`能够被应用于源码的哪些位置：

- 类或接口：`ElementType.TYPE`；
- 字段：`ElementType.FIELD`；
- 方法：`ElementType.METHOD`；
- 构造方法：`ElementType.CONSTRUCTOR`；
- 方法参数：`ElementType.PARAMETER`

#### @Retention

另一个重要的元注解`@Retention`定义了`Annotation`的生命周期：

- 仅编译期：`RetentionPolicy.SOURCE`；
- 仅class文件：`RetentionPolicy.CLASS`；
- 运行期：`RetentionPolicy.RUNTIME`。

如果`@Retention`不存在，则该`Annotation`默认为`CLASS`。因为通常我们自定义的`Annotation`都是`RUNTIME`

#### @Inherited

使用`@Inherited`定义子类是否可继承父类定义的`Annotation`。`@Inherited`仅针对`@Target(ElementType.TYPE)`类型的`annotation`有效，并且仅针对`class`的继承，对`interface`的继承无效

#### @Documented

生成API文档时包含该注解

```
import java.lang.annotation.*;

/**
 * 基础注解模板
 * 注解的核心要素：保留策略、作用目标、属性
 */
@Target({ElementType.TYPE, ElementType.METHOD}) // 可作用于类和方法
@Retention(RetentionPolicy.RUNTIME) // 运行时保留（可通过反射获取）
@Documented // 生成API文档时包含该注解
public @interface BaseAnnotation {
    // 注解属性（格式：类型 属性名() [default 默认值];）
    String value() default ""; // 默认属性（使用时可省略属性名）
    
    int order() default 0; // 排序序号
    
    boolean required() default false; // 是否必填
}
```



## IOC容器

（Inversion Of Control）**注解+反射**

- **核心思想**：传统开发中，对象的创建、依赖管理由开发者主动控制（如 `new UserService()`）；而 IoC 中，**对象的创建、依赖注入由 Spring 容器统一管理**，开发者只需定义 “需要什么”，无需关心 “如何创建”。
- 实现一个简化版的 IoC 容器，核心思路是**通过 “注解标识组件”+“反射创建对象”+“解析依赖并注入”**，替代手动 `new` 对象和管理依赖的过程。底层依赖**反射机制**（操作类和对象）和**注解解析**（识别需要管理的组件和依赖关系），核心步骤可概括为：**“扫描组件 → 解析依赖 → 初始化对象 → 注入依赖”**
- IoC 容器的底层核心是 **“通过注解 / 配置描述组件和依赖，通过反射自动创建对象并注入依赖”**，本质是用框架代码替代手动 `new` 对象和管理依赖的重复劳动，实现 “控制反转”（将对象创建权从开发者转移到容器）

## AOP

（Aspect Oriented Programming）**反射 + 动态代理**

- **核心思想**：将系统中 **通用、重复的逻辑（如日志、事务、权限校验、安全检查）** 抽取为 “切面（Aspect）”，在不修改业务代码的前提下，通过 “动态代理” 将切面逻辑织入到业务方法的指定位置（如方法执行前、执行后、异常时）。
- 使用注解实现AOP需要先定义注解，然后使用`@Around("@annotation(name)")`实现装配；

