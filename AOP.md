AOP

作用：在不惊动原始设计的基础上为其进行功能增强

核心：

 连接点（JoinPoint）

切入点（Pointcut）

通知（advise）

通知类

切面（aspect）

1.导入坐标（pom.xml）

```xml
     <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
            <version>3.3.1</version>
        </dependency>
```

2.制作共性功能（通知类与通知）绑定切入点与通知关系（切面）

```java
package com.example.javapratice.Aop;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Component;

@Component
@Aspect
public class MyAdvice {
    //在所有其他通知执行完毕后，环绕通知的结束部分被执行。这可以用于执行清理工作，或者在方法执行后修改返回值。
  @Around("execution(* com.example.javapratice.Service.PEService.*(..))")
    public Object method(ProceedingJoinPoint joinPoint)throws Throwable{
        
        System.out.println("Before method execution");

        Object result=joinPoint.proceed();//继续执行目标方法

        System.out.println("After method execution");

        return result;
    }
    

    //前置通知是在目标方法执行之前执行的通知，通常用于执行一些预处理任务，如日志记录、安全检查等。
     @Before("execution(* com.example.demo.aop.MyServiceImpl.performAction(..))")
    public void logBeforeAction() {
        System.out.println("Before performing action");
    }
    
      //独立于方法执行结果，后置通知总是会执行。这类似于在编程中的finally块，常用于资源清理。
  @After("execution(* com.example.demo.aop.MyServiceImpl.performAction(..))")
    public void logAfter() {
            System.out.println("After performing action");
  }
    
    //下面两个不常用
    
    //如果方法成功完成，即没有抛出异常，执行返回通知。这可以用来处理方法的返回值或进行某些后续操作.
    @AfterReturning(pointcut = "execution(* com.example.demo.aop.MyServiceImpl.performAction(..))",              returning = "result")
   public void logAfterReturning(Object result) {
    System.out.println("Method returned value is : " + result);
    }
    
    //如果方法执行过程中抛出异常，执行异常通知。这通常用于异常记录或进行异常处理
    @AfterThrowing(pointcut = "execution(* com.example.demo.aop.MyServiceImpl.performAction(..))", throwing       = "ex")
   public void logAfterThrowing(JoinPoint joinPoint, Throwable ex) {
    String methodName = joinPoint.getSignature().getName();
    System.out.println("@AfterThrowing: Exception in method: " + methodName + "; Exception: " +                 ex.toString());
  }
    
  
 
}

# () 参数说明
()匹配没有参数；  (..)代表任意多个参数；   (*)代表一个参数，但可以是任意型；    (*,String)代表第一个参数为任何值,第二个为String类型。
```

3.在Configuration类配置@EnableAspectJAutoProxy来启动@Aspect
