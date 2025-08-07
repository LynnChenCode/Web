# lombok库

## 1. `@Data`

**功能**：组合注解，等价于`@Getter + @Setter + @ToString + @EqualsAndHashCode + @RequiredArgsConstructor`的组合，适用于简单的 POJO 类。

```
@EqualsAndHashCode(callSuper = false)
```

```java
// 传统写法
public class User {
    private String name;
    private int age;
    
    // Getter
    public String getName() { return name; }
    public int getAge() { return age; }
    
    // Setter
    public void setName(String name) { this.name = name; }
    public void setAge(int age) { this.age = age; }
    
    // toString
    @Override
    public String toString() {
        return "User{name='" + name + "', age=" + age + "}";
    }
    
    // equals和hashCode
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        User user = (User) o;
        return age == user.age && Objects.equals(name, user.name);
    }
    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
    
    // 无参构造（@Data默认不包含，需手动添加或配合@NoArgsConstructor）
    public User() {}
}

// Lombok写法
import lombok.Data;
@Data
public class User {
    private String name;
    private int age;
}
```

## 2.构造方法

- `@NoArgsConstructor`：生成无参构造方法
- `@AllArgsConstructor`：生成包含所有字段的全参构造方法
- `@RequiredArgsConstructor`：生成包含`final`或`@NonNull`字段的构造方法

```java
// 传统写法
public class Order {
    private final String orderId; // final字段
    @NonNull private String userId; // 非null字段
    private String status;
    
    // RequiredArgsConstructor（包含final和@NonNull字段）
    public Order(String orderId, String userId) {
        this.orderId = orderId;
        this.userId = userId;
    }
    
    // NoArgsConstructor（需手动初始化final字段，否则编译报错）
    public Order() {
        this.orderId = "DEFAULT";
        this.userId = "DEFAULT";
    }
    
    // AllArgsConstructor
    public Order(String orderId, String userId, String status) {
        this.orderId = orderId;
        this.userId = userId;
        this.status = status;
    }
}

// Lombok写法
import lombok.*;
@RequiredArgsConstructor
@NoArgsConstructor
@AllArgsConstructor
public class Order {
    private final String orderId;
    @NonNull private String userId;
    private String status;
}
```



## 3.`@Slf4j`（日志注解）

**功能**：自动生成日志对象（`private static final Logger log = LoggerFactory.getLogger(类名.class);`），支持多种日志框架（如 SLF4J、Log4j 等）。

```java
// 传统写法
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
public class Service {
    private static final Logger log = LoggerFactory.getLogger(Service.class);
    
    public void doSomething() {
        log.info("执行操作");
    }
}

// Lombok写法
import lombok.extern.slf4j.Slf4j;
@Slf4j
public class Service {
    public void doSomething() {
        log.info("执行操作"); // 直接使用log对象
    }
}
```

## 4. `@Builder`

**功能**：为类生成建造者模式（Builder Pattern）代码，支持链式调用创建对象，适合字段较多的类。

```java
// 传统写法（建造者模式）
public class User {
    private String name;
    private int age;
    
    private User(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
    }
    
    public static class Builder {
        private String name;
        private int age;
        
        public Builder name(String name) {
            this.name = name;
            return this;
        }
        
        public Builder age(int age) {
            this.age = age;
            return this;
        }
        
        public User build() {
            return new User(this);
        }
    }
}

// 使用：new User.Builder().name("Alice").age(20).build();

// Lombok写法
import lombok.Builder;
@Builder
public class User {
    private String name;
    private int age;
}

// 使用：User.builder().name("Alice").age(20).build();
```

## 5. `@Cleanup`

**功能**：自动关闭实现了`AutoCloseable`接口的资源（如流、连接），替代`try-finally`代码块。

```java
// 传统写法
import java.io.FileInputStream;
public class FileUtil {
    public void readFile() throws Exception {
        FileInputStream fis = new FileInputStream("file.txt");
        try {
            // 读取文件操作
        } finally {
            if (fis != null) {
                fis.close();
            }
        }
    }
}

// Lombok写法
import lombok.Cleanup;
import java.io.FileInputStream;
public class FileUtil {
    public void readFile() throws Exception {
        @Cleanup FileInputStream fis = new FileInputStream("file.txt");
        // 读取文件操作（fis会自动关闭）
    }
}
```

## 6.其它

### 6.1依赖注入对比

- **必需依赖用构造注入**：用`final`修饰，通过构造方法注入，保证必须被初始化（否则编译报错）,强制初始化，保证核心功能可用，符合 “显式依赖原则”。
- **可选依赖用 @Autowired 的 setter 注入**：灵活处理 “有则用，无则不用” 的场景，不影响核心功能。

#### @RequiredArgsConstructor + final(构造方法注入的语法糖)（最优）

- 这种方式更符合 Spring 推荐的依赖注入规范构造方法注入优先于字段注入
- `@RequiredArgsConstructor`会为类中所有**被`final`修饰**或**被`@NonNull`标记**的字段生成对应的构造方法。
- Spring 在初始化 Bean 时，会自动查找并使用这个构造方法，将匹配的依赖注入进来（无需额外的`@Autowired`注解）。

#### 构造方法注入（手动）



#### @Autowired



```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class OrderService {
    // 必需依赖：订单数据库操作（没有则无法工作）
    private final OrderRepository orderRepository;
    
    // 可选依赖：短信通知服务（没有也能正常下单，只是不发通知）
    private SmsService smsService;

    // 构造注入：处理必需依赖（final修饰，强制初始化）
    public OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }

    // Setter方法 + @Autowired(required = false)：处理可选依赖
    // required = false 表示：如果Spring容器中有SmsService则注入，没有则忽略
    @Autowired(required = false)
    public void setSmsService(SmsService smsService) {
        this.smsService = smsService;
    }

    // 业务方法示例
    public void createOrder(Order order) {
        // 必需依赖：保存订单（核心功能）
        orderRepository.save(order);
        
        // 可选依赖：如果注入了则发送短信，否则跳过
        if (smsService != null) {
            smsService.sendNotification(order.getUserId(), "订单创建成功");
        }
    }
}
```



```java
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;

@Service
@RequiredArgsConstructor // 生成包含final字段的构造方法
public class UserService {
    // 用final修饰依赖，无需@Autowired
    private final UserDao userDao;
    private final OrderDao orderDao;
    
    // 业务方法
    public void queryUser() {
        userDao.query();
    }
}
```

### 6.2POJO类的链式调用对比

#### @Builder(最优)

```java
public class Main {
    public static void main(String[] args) {
        // 链式调用设置属性，最后通过build()创建实例
        User user = User.builder()
                .name("张三")
                .age(25)
                .email("zhangsan@example.com")
                .build();
        
        System.out.println(user); // 输出：User(name=张三, age=25, email=zhangsan@example.com)
    }
}
```



#### @Accessors

`@Accessors`（Accessor 的复数形式，意为 "访问器"）用于配置 getter/setter 的生成规则，其中`chain = true`是最常用的属性，它会让 setter 方法返回当前对象实例（`this`），而非默认的`void`，从而支持链式调用（如`obj.setName("a").setAge(18)`）

**其他常用属性**：

- `chain = true`：setter 方法返回当前对象（支持链式调用，如 `user.setName("a").setAge(10)`）。
- `fluent = true`：getter/setter 去掉 `get`/`set` 前缀（如`name()`代替`getName()`，`name("a")`代替`setName("a")`），且默认开启`chain = true`。
- `prefix = {"p", "m"}`：忽略字段前缀（如字段 `pName` 生成 `getName()` 而非 `getPName()`）。

```
// 传统写法（手动实现链式setter）
public class User {
    private String name;
    private int age;
    
    // 链式setter：返回当前对象
    public User setName(String name) {
        this.name = name;
        return this; // 关键：返回this
    }
    
    public User setAge(int age) {
        this.age = age;
        return this; // 关键：返回this
    }
    
    // getter
    public String getName() { return name; }
    public int getAge() { return age; }
}

// 使用：链式调用
User user = new User();
user.setName("Alice").setAge(20);


// Lombok写法（@Accessors(chain = true)）
import lombok.Setter;
import lombok.Getter;
import lombok.experimental.Accessors;

@Getter
@Setter
@Accessors(chain = true) // 开启链式setter
public class User {
    private String name;
    private int age;
}

// 使用：同样支持链式调用
User user = new User();
user.setName("Alice").setAge(20);
```

