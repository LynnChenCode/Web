```text
springboot-project
├── pom.xml                          # Maven项目配置（含Spring Boot Starter、Lombok等依赖）
├── src
│   └── main
│       ├── java
│       │   └── com
│       │       └── example
│       │           └── demo
│       │               ├── config                  # 配置类
│       │               │   ├── WebConfig.java      # @Configuration, @EnableWebMvc
│       │               │   ├── DataSourceConfig.java  # @Configuration, @ConfigurationProperties
│       │               │   └── SwaggerConfig.java  # @Configuration, @EnableSwagger2
│       │               │
│       │               ├── controller              # 控制层
│       │               │   └── UserController.java # @RestController, @RequestMapping
│       │               │
│       │               ├── service                 # 服务层
│       │               │   ├── UserService.java    # @Service
│       │               │   └── impl               # 实现类
│       │               │
│       │               ├── repository              # 持久层
│       │               │   └── UserRepository.java # @Repository (JPA) / @Mapper (MyBatis)
│       │               │
│       │               ├── model                   # 数据模型
│       │               │   ├── entity              # 实体类
│       │               │   │   └── User.java       # @Entity, @Table
│       │               │   ├── dto                 # DTO对象
│       │               │   └── vo                  # VO对象
│       │               │
│       │               ├── exception               # 异常处理
│       │               │   ├── GlobalExceptionHandler.java # @ControllerAdvice, @ExceptionHandler
│       │               │   └── BusinessException.java # 自定义异常
│       │               │
│       │               ├── aspect                  # AOP切面
│       │               │   └── LogAspect.java      # @Aspect, @Pointcut
│       │               │
│       │               ├── annotation              # 自定义注解
│       │               │   └── OperLog.java        # @Target, @Retention
│       │               │
│       │               ├── task                    # 定时任务
│       │               │   └── ScheduleTask.java   # @Scheduled, @EnableScheduling
│       │               │
│       │               └── util                    # 工具类
│       │                   └── JsonUtil.java       # @Component（可选）
│       │
│       └── resources
│           ├── application.yml        # 主配置（@Value读取配置项）
│           ├── application-dev.yml    # 开发环境配置
│           ├── application-prod.yml   # 生产环境配置
│           ├── logback-spring.xml     # 日志配置（SpringProfile支持）
│           ├── static                 # 静态资源
│           │   └── favicon.ico
│           ├── templates              # 模板文件
│           │   └── index.html
│           └── mapper                 # MyBatis映射文件
│               └── UserMapper.xml
│
├── target                             # 编译输出目录
│
└── src/test                           # 测试目录
    └── java
        └── com
            └── example
                └── demo
                    └── DemoApplicationTests.java  # @SpringBootTest, @MockBean
```

关键注解说明：

1. 配置层 (config)

- `@Configuration`: 声明为配置类(代替Spring容器的xml配置文件)
- `@ConfigurationProperties(prefix = "xxx")`: 绑定配置文件属性
- `@Enable*` 系列: 启用特定功能（如@EnableWebMvc, @EnableScheduling）

1. 控制层 (controller)

- `@RestController`: RESTful控制器
- `@RequestMapping`: 路由映射
- `@GetMapping`/`@PostMapping`: 请求方法映射
- `@RequestParam`/`@PathVariable`: 参数绑定

1. 服务层 (service)

- `@Service`: 服务组件标识
- `@Transactional`: 事务管理

1. 持久层 (repository)

- JPA: `@Repository` + 继承JpaRepository
- MyBatis: `@Mapper` 或 `@MapperScan`

1. 实体模型 (model)

- JPA实体: `@Entity`, `@Table`, `@Id`
- 参数校验: `@NotBlank`, `@Size`, `@Email`

1. 异常处理 (exception)

- `@ControllerAdvice`: 全局控制器增强
- `@ExceptionHandler`: 异常处理方法

1. AOP切面 (aspect)

- `@Aspect`: 声明切面类
- `@Pointcut`: 定义切点表达式
- `@Before`/`@Around`: 通知类型

1. 定时任务 (task)

- `@Scheduled`: 定义定时任务
- `@EnableScheduling`: 启用任务调度

最佳实践建议：

1. 分层结构清晰，遵循单一职责原则
2. 配置类集中管理，使用`@Profile`实现环境隔离
3. 业务异常统一处理，返回标准化响应
4. 使用DTO/VO隔离各层数据模型
5. 核心业务逻辑添加切面日志
6. 配置文件按环境拆分，敏感信息使用`jasypt`加密
7. 接口文档使用Swagger3 (`@OpenAPIDefinition`)