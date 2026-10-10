[§  项目预备知识](http://course.zhonghui.vip/training/)

http://course.zhonghui.vip/training/doc/proj4/proj_prep.html

# E商城项目开发预备知识  

　　Java语言目前在后端开发领域有着非常广泛的应用，尤其是大型互联网平台往往选择Java作为主要的后端编程语言。很多的大型网站或平台都是用Java做主要支撑的，例如淘宝、支付宝、京东、开源中国、中国移动等。同时，Java自身的生态比较健全，各种技术社群比较完善，有Spring、MyBatis、SpringBoot、SpringCloud等较为成熟的框架。

## 1 开发环境

- JDK
- Idea
- Tomcat

## 2 Java基础语法

### 2.1 第一个Java程序

```java
// Welcome.java
public class Welcome {
    public static void main ( String[] args )    {
        System.out.println("Welcome!");          
  }
}Copy
```

- 编译

  ```shell
  javac Welcome.java
  Copy
  ```

- 执行

  ```shell
  java Welcome
  Copy
  ```

### 2.2 变量和数据类型

#### 2.2.1 数据类型

Java中共三类8种基本数据类型：

- 数值型

  整型：byte(8位)、short(16位)、int（32位）、long（64位）

  浮点型：float(32位)、double(64位)

- 字符型：char

  字符型数据需要用单引号引起来，并且只能是一个字符。比如:

  ```
  '男'  '女'
  Copy
  ```

- 布尔型：boolean

  布尔类型只有true和false两个值。

#### 2.2.2 变量

- 声明变量

  ```java
  数据类型 变量名;
  Copy
  ```

  标识符：

  - 必须由字母、数字、下划线及美元符号组成；
  - 不能以数字开头；
  - 不能与关键字冲突；
  - 应该使用有意义的名称。

- 变量赋值

  使用赋值运算符直接给变量赋值即可。

  ```java
  变量名 = 值;
  Copy
  ```

#### 2.2.3 常量

常量中的值初始化以后，不能再修改。所以可以把程序的运行过程中不能改变的变量定义为常量。

```java
final 数据类型 常量名 = 常量值;//建议常量名全大写
Copy
```

#### 2.2.4 数据类型转换

- 字面值的问题

  - 整数字面值默认当作int类型，所以如果一个整数字面值超过了int能表示的数据范围，应该在数字后边加上l/L当作long类型。

    ```java
    long id = 120080221258L;
    Copy
    ```

  - 小数字面值默认当作double类型，如果想把一个小数当作float类型，需要在数字后边加上f/F。

    ```java
    float price = 12.58F;
    Copy
    ```

- 自动类型转换

  系统会自动将整型、浮点型、字符型的值从低级转换为对应的高级类型。

  从低级到高级：byte short(char) int long float double

- 强制类型转换

  反过来，从高级到低级就需要强制类型转换。

  ```java
  目标类型 变量 = (目标类型)数据;
  Copy
  ```

### 2.3 运算符

常用运算符有算术运算符、赋值运算符、比较运算符、逻辑运算符、条件运算符。

1. 算术运算符

算术运算符有+、-、*、/、%、++、--。

注意：算术运算时，同类型运算结果还是相同类型。如果两个运算数不同类型，一般会把低级自动转换为高级再同类型运算。

- 除 /

  ```java
  System.out.println(3/2);  //结果是整形1（丢弃小数位）
  System.out.println(3*1.0/2); //结果是1.5Copy
  ```

- 取余 % (取模)

- 自增，自减运算

  自增、自减运算一定作用于变量，给变量加1、减1。

  ++、--处于变量之前叫做前自增、前自减；处于变量之后之后叫做后自增、后自减。

  前后的区别主要在于运算结果不一样：

  - 前自增(减)是先给变量加(减)1，再把变量值作为运算结果。
  - 后自增(减)是先把变量值作为运算结果，再给变量加(减)1。

1. 赋值运算符

   赋值运算符有：=、+=、-=、*=、/=、%=。

2. 比较（关系）运算符

   比较运算符有：<、<=、>、>=、==、!=

   注意：比较运算的结果是boolean类型。

3. 逻辑运算符

   逻辑运算符：&、&&、||、|、！。

   - &和&&：两个操作数都为真，条件才为真。
   - |和||：两个操作数任何一个为真，条件为真。
   - ！：取反，如果条件为true，则逻辑非运算符将得到false。

&&和||的短路特性：

& 和 && 的区别：& 两边都运算，而 && 先算 && 左侧，若左侧为 false 那么右侧就不运算了。而 | 和 || 的比较与上类似。

1. 条件运算符

   条件运算符主要用来通过判断条件的真假决定两个表达式中哪个作为运算结果。

   ```java
   op1?op2:op3
   Copy
   ```

   op1 的结果必须为布尔类型，如果 op1 的值为 true，则表达式最终的结果为 op2；如果 op1 的值为 false，则表达式最后的结果为 op3。

   ```java
   //处理金额price变量，如果金额小于0当0处理。
   int price = -1;
   price = price>=0?price:0;
   System.out.println(price);//0Copy
   ```

### 2.4 流程控制结构

#### 2.4.1 选择结构

- 单分支的if 语句

  ```java
  if (表达式) {
      语句块;
  }Copy
  ```

  表达式为true，执行语句块；否则不执行语句块;

- if-else语句

  ```java
  if (表达式) {
      代码块1;
  }else{
      代码块2
  }Copy
  ```

  表达式为true，执行代码块1；否则执行代码块2；

- 多if语句

  ```java
  if(条件表达式 1){
      代码块 1;
  }else if(条件表达式 2){
      代码块 2;
  
  }else{
      代码块 n;
  }Copy
  ```

  如果表达式1结果为true，就执行代码块1；

  否则 如果提交表达式2结果为true，就执行代码块2；否则执行代码块n。

- switch

  ```java
  swtich(表达式 )
  {
      case 常量 1：
          语句 1；
          break；
      case 常量 2：
          语句 2；
          break；
          …
      case 常量 n：
          语句 n；
          break；
      default:
          语句;
  }Copy
  ```

  多重if-else语句较适用于区间范围的比较，switch语句适合等值比较。

#### 2.4.2 循环结构

- while 语句

  ```java
  while(循环条件){
      循环内容;
  }Copy
  ```

- do...while

  ```java
  do{
      循环内容（包含改变初始值）;
  }while(循环条件); //别忘记分号Copy
  ```

- for

  ```java
  for(变量初始化;循环条件;更改变量){
      循环内容;
  }Copy
  ```

  ```java
  //打印斐波那契数列（前10）
  int n1=1;
  int n2=1;
  for(int i=1;i<=10;i++){
      if(i<=2){
          System.out.println(1);
      }else{
          System.out.println(n1+n2);
          n2=n1+n2;
          n1=n2-n1;
      }
  }Copy
  ```

- 循环结构比较

  while和for 是先判断后执行，执行次数是0~n次；do-while是先执行后判断，执行次数是1~n次。while和do-while更适用于循环次数不确定，for循环更适用于循环次数确定。

- break

  break 语句用于终止某个语句块的执行，可以使用在 while、do-while、for、switch 语句体中，用在循环语句体中，可以强行结束循环。

- continue

  continue 适用于任何循环控制结构中。作用是让程序立刻跳转到下一次循环的迭代。

### 2.5 数组

数组分为一维数组和多维数组，数组属于引用数据类型。

- 声明数组

  数组元素类型[ ] 数组名；

  数组元素类型 数组名[ ]；

- 分配空间

  变量名 = new 类型[元素个数]

- 数组元素访问

  数组名[索引]

  索引：从0到length-1;

  数组.length是数组的长度。

  ```java
  int[] arr = new int[3];
  arr[0] = 0;
  arr[1] = 1;
  arr[2] = 2;Copy
  ```

- 声明、分配空间，初始化同时进行

  ```java
  int[] arr2 = {1,2,3};//声明的同时初始化
  int[] arr3;
  arr3 = new int[]{4,5,6};//声明以后，再初始化
  for(int n : arr3){
      System.out.println(n):
  }Copy
  ```

- 二维数组

  数组的元素还是数组。

  ```
  int[][] arr = new int[5][];//只分配二位数组空间
  int[][] arr2 = new int[5][6];//二位数组空间和包含的n个一位数组空间同时分配Copy
  ```

  ```java
  //杨辉三角形
          final int ROWS = 5;
          int[][] arr = new int[ROWS][];
          /**
              1
             1 1
            1 2 1
           1 3 3 1
          1 4 6 4 1*/
          /**
          1
          11
          121
          1331
          14641*/
          //给每行分配空间，并初始化每行第一个和最后一个1
          for(int i=0;i<arr.length;i++) {
              arr[i] = new int[i+1];
              arr[i][0] = 1;
              arr[i][i] = 1;
          }
          //给每行中间的每位计算值
          for(int i=0;i<arr.length;i++) {
              for(int j=1;j<arr[i].length-1;j++) {
                  arr[i][j] = arr[i-1][j-1]+arr[i-1][j];
              }
          }
          //打印
          for(int i=0;i<arr.length;i++) {
              //输出空格
              for(int j=0;j<arr.length-1-i;j++) {
                  System.out.print(" ");
              }
              //输出数字
              for(int j=0;j<arr[i].length;j++) {
                  System.out.print(arr[i][j]+" ");
              }
              System.out.println();
          }Copy
  ```

## 3 Java面向对象

### 3.1 Java类和对象

对象是真实存在的实体，具有状态（属性）和行为（方法），类是一个模板，它描述一类对象的行为和状态，也可以理解类为具有相同状态和行为的一组对象的集合。

从对象到类是抽象的过程，从类到对象是实例化的过程。

使用面向对象的思想来编写程序，是先从众多对象中抽象出来类（创建类），然后根据类实例化对象，通过对象调用其方法和属性来实现具体功能。

- 类

  类是一种对象模版，它定义了如何创建实例，因此，类本身就是一种数据类型：

  类中只包含成员属性（成员变量）和成员方法。

  ```java
  [修饰符] class 类名{
      属性定义
      方法定义
  }Copy
  ```

  ```java
  public class Employee{
    //属性声明
      String name;
      //方法声明
      public void print(){
      }
  }Copy
  ```

  类名首字母大写，其他规则与声明变量一致。

- 对象

  对象是根据类创建的实例，可以创建多个instance，每个instance类型相同，但各自属性可能不相同：

  ```java
  类名 对象名 ＝ new 类名();
  Copy
  ```

  ```java
  Employee e = new Employee();
  Copy
  ```

  ***实例化对象以后，就可以调用对象属性和方法：\***

  ```java
  对象名.属性名;
  对象名.方法名(实参列表);Copy
  ```

```java
  e.name = "admin";
  e.print();Copy
```

### 3.2 方法

- 方法

  #### ***方法的定义\***

  ```java
  [修饰符] 返回值类型 方法名(参数列表){
      // 方法体
  }Copy
  ```

  **·** ***修饰符：\***修饰符，这是可选的，定义了该方法的访问类型。

  **·** ***返回值类型 ：\***返回值类型分为void和具体数据类型，void表示没有返回具体数据，如果有返回值，需要使用return关键字。

  **·** ***方法名：\***是方法的实际名称。方法名的命名规范是首字母小写，使用驼峰式且有意义。

  **·** ***参数\***：参数列表是指方法的参数类型、顺序和参数的个数。参数是可选的，方法可以不包含任何参数。

  **·** ***方法体：\***方法体包含具体的语句，定义方法的功能。

  ```java
  public void print(){
      System.out.println();
  }Copy
  ```

  #### 方法的调用

  方法定义后，若想执行功能则进行调用。

  ```java
  对象.方法名([参数]);
  Copy
  ```

  ```java
  e.print();
  Copy
  ```

- 构造方法

  构造方法是一种特殊的方法，它是一个与类同名且没有返回值的方法。对象的创建就是通过构造方法来完成的，其功能主要是完成对象的初始化。当类实例化一个对象时会自动调用构造方法。

  每创建一个类，如果不显示的创建构造方法，系统都会提供一个默认无参的构造方法，形如：

  ```java
  public Employee(){
  }Copy
  ```

  如果想创建对象的同时给属性初始化，则可以创建有参数的构造方法。

  ```java
  public Employee(String name,int age,double salary){
      this.name = name;
      this.age = age;
      this.salary = salary;
  }Copy
  ```

- this

  在方法中，this代表当前对象。通过this.就可以访问当前对象的成员变量或者方法。

  **this.**可以省略，但是以下情况不可以省略。

  - 如果方法参数名和成员变量名相同，需要显式使用this.进行区分。

  - 如果需要在一个构造中调用另一个构造

    ```java
    public Employee(){
        this("test",0,0.0);
    }
    public Employee(String name,int age,double salary){
        this.name = name;
        this.age = age;
        this.salary = salary;
    }Copy
    ```

- static

  类成员和对象成员，默认成员变量和方法都属于对象；如果成员变量或者方法前加上static修饰，那么该成员就属于类。

  static修饰的成员变量或者方法叫做类成员或者静态成员。

  类成员直接通过**类名.**就可以访问.

  ```java
  class Flags{
      public static int flag_ok = 0;
      public static int ok(){
          return flag_ok;
      }
  }
  Flags.ok();Copy
  ```

### 3.3 封装

封装是防止该类的代码和数据被外部类定义的代码随机访问，增强了代码的安全性和合理性。

- 封装粒度

  - 函数

  - 对象

    - private私有化成员
    - public方法对外提供访问接口

    ```java
    public class Employee{
        private String name;
        private int age;
        private double salary;
        public String getName(){
            return this.name;
        }
        public void setName(String name){
            this.name = name;
        }
        public int getAge(){
            return this.age;
        }
        public void setAge(){
            this.age = age;
        }
        public double getSalary(){
            return this.salary;
        }
        public void setSalary(){
            this.salary = salary;
        }
    }Copy
    ```

  - 组件

  - 平台

- 访问修饰词

  | 修饰词 | 类内部 | 同一个包 | 子类 | 任何地方 | | --------- | ------ | -------- | ---- | -------- | | private | Y | | | | | default | Y | Y | | | | protected | Y | Y | Y | | | public | Y | Y | Y | Y |

- 包

  以树状结构对类进行组织。

  - package

    ```java
    package com.it.domain;
    public class Employee{
    
    }Copy
    ```

    ```java
    package com.it.service;
    public class EmployeeService{
    
    }Copy
    ```

  - import

    加了package包名以后，访问一个类时应该使用完整类名：包名.类名 (同包中可以省略包名.)

    方便起见，也可以先用import导入一个类，然后访问该类就可以直接使用简单类名。

    ```java
    package com.it.test;
    import com.it.service.EmployeeService;
    public class EmployeeTest{
        public static void main(String[] args){
            com.it.domain.Employee e = new com.it.domain.Employee();
            EmployeeService es = new EmployeeService();
        }
    }Copy
    ```

### 3.4 继承

- 继承

  为了减少冗余代码，使代码可以复用，Java使用继承来设计类和类的关系。

  继承的思路就是将共同的属性和方法抽象成父类，让子类通过继承的方式，达到可以拥有父类的属性和方法，除此之外，子类还可以根据自身的特点定义特有的属性和方法。子类和父类必须是is-a的关系，比如，麻雀（子类）是一种鸟（父类）。

  ```
  [修饰符] class 子类名 extends 父类名{类体定义 }
  Copy
  ```

  ```java
  public class Person {
      private String name;
      //省略getter/setter方法
  }
  public class Teacher extends Person{   //继承了父类的属性和方法
      private int teacherId;  //特有的属性
      public int getTeacherId() {  //特有的方法
      }
      public void setTeacherId(int teacherId) {
      }
  }Copy
  ```

  ***没有显式指定父类的类，其父类默认是Object\***

- 重写

  当子类继承的父类方法不能满足需求时，需要进行重写方法，也叫方法覆盖。

  方法重写的规则：

  - 方法名和参数列表相同;
  - 返回值相同或是其子类;
  - 访问权限一定要高于或等于父类方法的访问权限 。

- super

  super标示父类对象，使用super.可以访问到继承自父类的成员。

  super.可以省略,但是在以下情况不能省略。

  - 子类中声明了和父类种同名的属性或者重写了父类方法，这时需要通过super.去显式访问继承自父类的成员。

  - 子类构造中调用父类构造时，需要使用super

    ```java
    public class Student extends Person{   //继承了父类的属性和方法
        private int stuno;  //特有的属性
        public Student(){
            super();//调用父类构造
        }
    }Copy
    ```

### 3.5 多态

多态是现实事物经常会体现出多种形态，在程序中多态是指声明父类的引用，因为引用了不同子类的对象，从而表现出不同子类对象的状态。比如Person p;这个父类引用，它如果引用一个new Teacher();对象那么表现出的就是Teacher的形态；而如果引用一个new Student();对象那么表现出的就是Student的形态。

- 上转型

  父类引用子类对象就叫做向上转型。（也就是把子类对象当作父类类型去使用）

  ```java
  Person p = new Teacher();
  p = new Student();Copy
  ```

  注意：

  - 向上转型以后，只能访问在父类中声明过的成员。不能访问子类特有的成员。
  - 向上转型以后，如果调用方法时，方法在子类中有被重写，那么调用的是子类重写的方法。

- 下转型

  下转型就是把上转型以后的对象应用又强制转换为子类类型。

  下转型是强制类型转换，不安全，所以建议先判断，再转型。

  ```java
  if(p instanceOf Student){
      Student stu = (Student)p;
  }Copy
  ```

### 3.6 抽象和接口

- 抽象

  我们常说父类比较抽象，子类比较具体。而父类抽象到一定程度就无法实例化对象了，并且有些方法就无法实现了，这时可以把类声明为抽象类，把方法声明为抽象方法。

  - 抽象类

    ```java
    public abstract class Person{}
    Copy
    ```

    ***抽象类不能实例化对象（就用来被子类继承）\***

  - 抽象方法

    ```java
    public abstract class Person{
        public abstract void fun();
    }Copy
    ```

    **抽象方法没有实现（到子类中再去实现），并且抽象方法必须在抽象类中**

- 接口

  Java 接口是一系列方法的声明，是一些方法特征的集合，接口是解决 Java 无法使用多重继承的一种手段，但是接口在实际中更多的作用是制定标准。

  接口与类的区别：

  - 接口关键字interface
  - 接口中只能声明静态常量(成员变量)和抽象方法（所以可以把接口看作特殊的抽象类）
  - 接口和接口可以继承，并且时多结成
  - 类可以实现(implements)接口，也是多实现。

  ```java
  interface EmployeeService{
      String LABEL = "EMP";//默认就是public static final
      void createEmployee(Employee e);//默认就是public abstract
      void updateEmployee(Employee e);
  }
  class EmployeeServiceImpl implements EmployeeService{
      public void createEmployee(Employee e){        
      }
      public void updateEmployee(Employee e){        
      }
  }
  EmployeeService es = new EmployeeServiceImpl();Copy
  ```

## 4 Java常用类

### 4.1 包装类

- Integer、Long、Byte、Short
- Double、Float
- Boolean
- Character

```java
String numStr = "12";
int num = Integer.parseInt(numStr);Copy
```

### 4.2 String和Date

- String
- Date

### 4.3 集合Collection和映射Map

![img](https://www.runoob.com/wp-content/uploads/2014/01/2243690-9cd9c896e0d512ed.gif)

#### 4.3.1 Collection

- Set
  - Set 接口实例存储的是无序的，不重复的数据。
- List
  - List 接口实例存储的是有序的，可以重复的元素。
  - List 可以动态增长，根据实际存储的数据的长度自动增长 List 的长度。

#### 4.3.2 Map

Map接口用于组织一组成对的“键值对”对象，每个元素包括两部分：键（key）和值（value），键（key）不能重复，值（value）可以重复。

```java
        Map<String,String> m = new HashMap<>();
        m.put("CN", "中国");
        m.put("US", "美国");
        m.put("RUS", "俄罗斯");
        Set<Entry<String, String>> entrySet = m.entrySet();
        Iterator<Entry<String, String>> it = entrySet.iterator();
        while(it.hasNext()) {
            Entry<String, String> e = it.next();
            String key = e.getKey();
            String value = e.getValue();
            System.out.println(key+":"+value);
        }Copy
```

## 5. Java数据库访问

### 5.1 JDBC常用接口和类

***DriverManager类\***

DriverManager(驱动程序管理器)类，负责管理JDBC驱动程序。

常用方法：

public static synchronized Connection getConnection(String url,String user,String password)

***Connection接口\***

Connection对象代表与数据库的连接，通过DriverManager.getConnection()方法获得。

常用方法：

| ***方法\***                                                  | ***作用\***                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Statement createStatement() throws SQLException              | 创建Statement对象,用于执行不带占位符的SQL语句                |
| PreparedStatement prepareStatement(String sql) throws SQLException | 创建Preparestatement对象，能把SQL语句（一般带有占位符）进行预编译以提高执行效率 |
| CallableStatement prepareCall(String sql)                    | 创建CallableStatement 对象，用来调用数据库存储过程           |

***Statement接口\***

用来执行静态SQL语句，及不带占位符的SQL语句。

常用方法：

| ***方法\***                                            | ***作用\***                          |
| ------------------------------------------------------ | ------------------------------------ |
| ResultSet executeQuery(String sql) throws SQLException | 执行一个查询语句并返回结果集         |
| int executeUpdate(String sql) throws SQLException      | 执行更新操作，返回影响的行数         |
| boolean execute(String sql) throws SQLException        | 执行更新或查询语句，返回是否有结果集 |

***ResultSet\***

Statement执行查询时得到的查询结果集，ResultSet接口提供了行遍历及列遍历访问结果集的方法。

常用方法：

| ***\*方法\****                     | ***\*作用\****                                               |
| ---------------------------------- | ------------------------------------------------------------ |
| boolean next() throws SQLException | 把当前指针定位到下一行。注意，最初，ResultSet的指针位于第一行之前。 |
| Object getXxx(int index)           | 根据索引值获取当前行中某列的值                               |
| Object getXxx(String colName)      | 根据字段名获取当前行中某列的值                               |

其中“Xxx”与列的数据类型有关，例如，

如要获取的列是String类型，则使用getString()方法获取该列的值。

### 5.2 JDBC访问数据库

***Statement与PreparedStatement\***

PreparedStatement是Statement的子接口。

Statement可以执行基本的SQL，但

- 执行静态语句简便，若执行动态语句，使用“+”拼接麻烦

- 批量执行结构相同的SQL语句时，效率较低

  PreparedStatement可以防止SQL注入

- 在将SQL语句发送到数据库之前，预先进行编译

- SQL语句中可以包含参数，在执行时动态指定这些参数

- SQL语句使用“?”作为数据占位符

- 按索引位置，使用setXxx()方法设置数据

***PreparedStatement的常用方法：\***

| ***方法\***                                  | ***作用\***                  |
| -------------------------------------------- | ---------------------------- |
| ResultSet executeQuery() throws SQLException | 执行一个查询语句并返回结果集 |
| int executeUpdate() throws SQLException      | 执行更新操作，返回影响的行数 |

- 导入依赖包

  ![image-20220705162750833](http://course.zhonghui.vip/training/doc/proj4/images/mysql.png)

  注意要添加到构建路径：

  ![image-20220705162928825](http://course.zhonghui.vip/training/doc/proj4/images/buildpath.png)

- 代码示例

```java
public class DBUtil {
    static{
        //加载数据库驱动，只加载一次
        try {
            Class.forName("com.mysql.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
    /**
     * 获取连接
     * @return 连接
     * @throws SQLException
     */
    public static Connection getConnection() throws SQLException{
        String url = "jdbc:mysql://localhost:3306/test";
        Connection conn = DriverManager.getConnection(url,"root","123456");
        return conn;
    }

    /**
     * 释放资源
     * @param rs 结果集
     * @param st Statement对象
     * @param conn 连接
     */
    public static void close(ResultSet rs, Statement st, Connection conn) {
        try {
            if(rs!=null) {
                rs.close();
            }
            if(st!=null) {
                st.close();
            }
            if(conn!=null) {
                conn.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }    
}Copy
```

## 6 Java Web 应用开发

平时上网的时候，打开浏览器（客户端）访问网址，此时会向网址所在的服务器上发送请求，服务器获得请求后会将请求的数据响应给发送请求的浏览器（比如：html，css，javascript等），服务器的主要作用之一是在网络环境中提供外界可以访问的资源。在服务器中提供对外界访问的资源一般分为两种：

- 静态资源：如html,css,javascript等，指提供给客户端浏览的数据内容不会改变。
- 动态资源：如servlet、JSP等，指提供给客户端浏览的数据由程序动态生成，不同的客户端浏览到的数据可能有所不同。

![客户端和服务器](http://www.monkey1024.com/wp-content/uploads/2017/09/%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%92%8C%E6%9C%8D%E5%8A%A1%E5%99%A8.png)

### 6.1 第一个Web项目

- eclipse集成tomcat

  打开eclipse，选择界面下方的servers选项，点击no servers are available，click this link to create a new server。 选择Apache—>tomcat v9.0 server之后选择tomcat的解压目录点击finish即可。注意版本号要一致。

- 新建动态web工程

  在eclipse中创建一个dynamic web项目，在WebContent目录下创建一个index.html里面随意编写内容。

- 发布部署

  在eclipse中创建一个dynamic web项目，在WebContent目录下创建一个index.html里面随意写点内容。 在servers选项中右键tomcat，选择add and remove，在弹出的对话框中选择要添加的项目名称将其添加到右侧，点击finish。在tomcat上右键点击start启动tomcat。 tomcat启动成功之后，在浏览器中输入：http://localhost:8080/firstweb/ 就可以访问之前编写的index.html页面了，这样就成功的发布了一个web项目。

  注意：firstweb是应用部署的上下文路径，默认就是项目名。（生产环境一般是空）

### 6.2 Servlet

servlet是一门用于开发动态web资源的技术，可以运行在Web服务器中的小型Java程序，有时也叫做服务器端的小应用程序，servlet是server applet两个单词的组合而成。servlet 可以通过 HTTP协议接收和响应来自 Web 客户端的请求。

- 第一个Servlet

  在项目创建一个Java类实现servlet接口并重写里面的方法。

  ```java
  public class FirstServlet implements Servlet {
      @Override
      public void service(ServletRequest arg0, ServletResponse arg1) throws ServletException, IOException {
          System.out.println("Hello Servlet");
      }
  
      @Override
      public void destroy() {
  
      }
      @Override
      public ServletConfig getServletConfig() {
          return null;
      }
      @Override
      public String getServletInfo() {
          return null;
      }
      @Override
      public void init(ServletConfig arg0) throws ServletException {
  
      }
  }Copy
  ```

  servlet创建好之后，需要在web.xml文件中进行配置，给servlet一个可以访问的URI地址(也可以通过@WebServlet注解)：

  ```xml
  <!-- 创建一个servlet实例 -->
  <servlet>
      <!-- 给servlet取一个名字，该名字需与servlet-mapping中的servlet-name一致 -->
    <servlet-name>firstServlet</servlet-name>
      <!-- servlet的包名+类名 -->
      <servlet-class>com.monkey1024.servlet.FirstServlet</servlet-class>
    </servlet>
  
    <!-- 给servlet一个可以访问的URI地址 -->
    <servlet-mapping>
      <!-- servlet的名字，与 servlet中的servlet-name一致-->
      <servlet-name>firstServlet</servlet-name>
      <!-- URI地址:http://locahost:8080/07-01-servlet/hello -->
      <url-pattern>/hello</url-pattern>
    </servlet-mapping>Copy
  ```

  之后将该web项目部署到tomcat中，启动成功后访问：http://locahost:8080/firstweb/hello 可以看到eclipse控制台中打印出了Hello Servlet 通过上面示例可以看出，servlet的需要部署在tomcat中才能运行，有时tomcat也被称为是servlet的容器。

***Servlet执行流程\***

![servlet访问流程图](http://course.zhonghui.vip/training/doc/proj4/images/servlet%E8%AE%BF%E9%97%AE%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

**Servlet生命周期**

![servlet生命周期](http://course.zhonghui.vip/training/doc/proj4/images/servlet%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)

- 第一次请求一个Servlet时，会先实例化并初始化Servlet对象。(通过配置load-on-startup在服务启动时初始化Servlet)

  - 每次请求一个Servlet时，Servlet都会提供一次服务，也就是调用一次service方法。

- Servlet对象时单例，在服务停止时才会销毁。

- HttpServlet

  在实际应用中常用的http提交方式有get和post（除此之外还有put、delete），在之前所编写的servlet中是无法直接处理这两种提交方式的，为了方便开发，JavaEE规范的API提供了javax.servlet.http.HttpServlet类，在实际开发中也经常使用继承HttpServlet类的方式创建一个servlet。

  ```java
  /**
   * 继承HttpServlet处理get和post请求
   *
   */
  public class HttpTest01 extends HttpServlet {
  
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          System.out.println("执行doGet方法");
      }
  
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          System.out.println("执行doPost方法");
      }
  }Copy
  ```

- request

  Web服务器收到客户端的http请求，会针对每一次请求，创建一个用于代表请求的HttpServletRequest类型的request对象，并将"HTTP请求协议"的完整内容封装到该对象中。开发者获拿到request对象后就可以获取客户端发送给服务器的请求数据了。

  ***HTTP协议请求信息格式\***

  ```http
  POST https://edu.bitejiuyeke.com/tms/login HTTP/1.1
  Host: edu.bitejiuyeke.com
  Connection: keep-alive
  Content-Length: 117
  sec-ch-ua: " Not A;Brand";v="99", "Chromium";v="99", "Microsoft Edge";v="99"
  sec-ch-ua-mobile: ?0
  User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.74 Safari/537.36 Edg/99.0.1150.52
  Access-Control-Allow-Methods: PUT,POST,GET,DELETE,OPTIONS
  Content-Type: application/json;charset=UTF-8
  Access-Control-Allow-Origin: *
  Accept: application/json, text/plain, */*
  Access-Control-Allow-Headers: Content-Type, Content-Length, Authorization, Accept, X-Requested-With , yourHeaderFeild
  sec-ch-ua-platform: "Windows"
  Origin: https://edu.bitejiuyeke.com
  Sec-Fetch-Site: same-origin
  Sec-Fetch-Mode: cors
  Sec-Fetch-Dest: empty
  Referer: https://edu.bitejiuyeke.com/login
  Accept-Encoding: gzip, deflate, br
  Accept-Language: zh-CN,zh;q=0.9
  
  {"username":"132424328182","password":"Zo/BAcYpgdfsgsMg9yPf/nM2g==","uuid":"e04184dbda5d45ad9991b2a8b1dbd639","status":0}
  Copy
  ```

  ***HttpServletRequest中常用的方法:\***

  - 首行

- 协议头

  - 正文

  ***请求参数处理：\***

  - Map getParameterMap() 获取包含所有请求参数及值的 Map 对象。需要注意，该 Map 的 value 为 String[]，即一个参数所对应的值为一个数组。说明一个参数可以对应多个值。

  - Enumeration getParameterNames() 获取请求参数 Map 的所有 key,即获取所有请求参数名。

  - String[] getParameterValues(String name) 根据指定的请求参数名称，获取其对应的所有值。这个方法一般用于获取复选框(checkbox)数据。

  - String getParameter(String name) 根据指定的请求参数名称，获取其对应的值。若该参数名称对应的是多个值，则该方法获取到的是第一个值。这个方法是最常用的方法。

  - 前端提交参数

    ```html
    <form action="createuser" >
        用户名：<input name="username" type="text">
        <br>
        密码：<input name="password" type="password">
        <br>
        爱好：
        <input type="checkbox" name="hobby" value="basketball">篮球
        <input type="checkbox" name="hobby" value="football">足球
        <input type="checkbox" name="hobby" value="volleyball">排球
        <br>
        <input type="submit" value="提交">
    </form>Copy
    ```

  - 后端读取参数

    ```java
    /**
     * HttpServletRequest获取请求数据
    */
    public class RequestTest01 extends HttpServlet {
        private static final long serialVersionUID = 1L;
    
        protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            //根据html中的name的名字获取用户在input中填写的值
            String username = request.getParameter("username");
            String password = request.getParameter("password");
            //获取用户勾选的checkbox的值
            String[] hobby = request.getParameterValues("hobby");
    
            System.out.println(username);
            System.out.println(password);
            for(String s:hobby){
                System.out.println(s);
            }
        }
    
        protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            doGet(request, response);
        }
    }Copy
    ```

参数中文乱码处理：

- 正文参数(post/put)

  ```java
        //在读取参数之前设置字符编码
      request.setCharacterEncoding("utf-8");Copy
  ```

- URI参数(get/delete)

  - 第一种方式：先按照默认编码读出来，再重新编解码

  ```java
      //根据html中的name的名字获取用户在input中填写的值
  String username = request.getParameter("username");
      //将数据按照ISO8859-1编码后放到字节数组中
    byte[] bytes = username.getBytes("ISO8859-1");
      //将字节数组按照UTF-8解码为字符串
    username = new String(bytes,"UTF-8");Copy
  ```

  

  - 第二种方式：直接在tomcat配置文件server.xml中设置URI编码字符集

    ```xml
      <Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443" URIEncoding="UTF-8"/>
    Copy
    ```

- response

  Web服务器收到客户端的http请求，会针对每一次请求，创建一个用于代表响应的HttpServletResponse类型的response对象，开发者可以将要向客户端返回的数据封装到response对象中。

  ***HTTP协议响应信息格式\***

  ```http
  HTTP/1.1 200 OK
  Server: nginx/1.9.9
  Date: Wed, 08 Dec 2021 12:43:05 GMT
  Content-Type: text/css
  Transfer-Encoding: chunked
  Connection: keep-alive
  
  <!DOCTYPE html><!--STATUS OK-->
  <html><head><meta http-equiv="Content-Type" content="text/html;charset=utf-8"><meta http-equiv="X-UA-...........................
  Copy
  ```

  ***HttpServletResponse中常用的方法:\***

  - 首行

- 协议头

  - 正文

  **HttpServletResponse响应：**

  - setCharacterEncoding(String charset)

  设置输出内容使用的字符编码(必须在getWriter之前)

  - setContentType(String contentType)

    设置Content-Type消息头

  - PrintWriter getWriter()

    获取用来输出文本内容的字符流。(也可以获取字节流)

  ```java
        response.setCharacterEncoding("UTF-8");
          //从response中取得PrintWriter对象
        PrintWriter out = response.getWriter();
          //向客户端发送数据
          out.print("用户：" + username + "添加成功！<br>");
          out.print("感谢您的注册");Copy
  ```

### 6.3 JSP

JSP全称是Java Server Pages，是一种动态网页技术，JSP其实就是在html中插入了java代码和JSP标签之后形成的文件，文件名以.jsp结尾。其实JSP就是一个servlet。 在servlet中编写html比较痛苦，而写JSP就像在写html，但它相比html而言，html只能为用户提供静态数据即静态页面，而Jsp技术允许在页面中嵌套java代码，为用户提供动态数据，从而形成动态页面。需要注意的是最好只在JSP中编写动态输出的java代码。

- 第一个JSP

  在WebContent目录下点击右键—>new—>JSP file，创建一个名为first.jsp的文件。

  ```jsp
  <%@ page language="java" contentType="text/html; charset=utf-8"
      pageEncoding="utf-8"%>
  <%@ page import="java.util.*" %>
  <!DOCTYPE html>
  <html>
  <head>
  <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
  <title>第一个JSP</title>
  </head>
  <body>
      <%
          Date d = new Date();
          out.write(d.toLocaleString());
      %>
  </body>
  </html>Copy
  ```

  直接按照文件路径访问jsp：http://localhost:8080/demo03/first.jsp

  jsp也可以想Servlet一样配置一个url路径。

- JSP语法组成

  - 静态文本

    静态文本直接作为字符串输出，比如jsp中的静态html内容。

  - 指令

    ```jsp
    <%@ 指令名  指令相关属性 %>
    Copy
    ```

    - page
    - taglib
    - include

  - java表达式

    ```jsp
    <%=java表达式%>
    Copy
    ```

    java表达式的结果直接输出。一般用来输出动态数据。

  - java代码块

    ```jsp
    <%java代码块%>
    Copy
    ```

    java代码块原样复制到service方法中正常执行。

  - java声明

    ```
    <%!java声明%>
    Copy
    ```

    java声明代码原样复制到servlet类中作为servlet类的成员。

  - jsp标签

    ```jsp
    <jsp:标签名></jsp:标签名
    Copy
    ```

    - jsp:forward
    - jsp:include

  - el表达式

    el表达式可以代替java表达式输出动态数据。

    ```jsp
    ${el表达式}
    Copy
    ```

    - 运算符

      - 算术、关系(empty)、逻辑、条件

      - 对象(map)属性访问

        ```
        对象["属性名"]
        对象.属性名Copy
        ```

      - 数组(列表)元素访问

        ```jsp
        数组[下标]
        Copy
        ```

    - 操作数

      - 字面值

      - 变量

        从4个域中按照顺序查找

      - 11个隐式对象

  - JSTL标签

    JSTL标签可以代替java代码块实现一些逻辑处理，比如选择或者循环。

    - core核心库
      - c:if
      - c:forEach
    - format格式化库
    - 等等

    ```
    1.下载标签库
    jstl-1.2.jar
    下载地址：http://tomcat.apache.org/taglibs/standard/
    2.放置Web应用WEB-INF/lib目录下
    3.引入标签库
    例如：
    <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>Copy
    ```

  - 注释

    ```jsp
    <%-- --%>
    Copy
    ```

  ```jsp
  <br>---------------------遍历List--------------------------<br>
  <%
      List<String> name = new ArrayList<>();
      name.add("刘德华");
      name.add("张学友");
      name.add("黎明");
      name.add("郭富城");
      pageContext.setAttribute("name", name);
  %>
  <c:if test="${empty name}">
      没有数据!!!
  </c:if>
  <c:forEach items="${name }" var="n">
      ${n }
      <br>
  </c:forEach>Copy
  ```

### 6.4 转发和重定向

服务器处理完客户端的请求都需要跳转，跳转分为转发和重定向。

- 转发

  RequestDispatcher对象用来将客户发送的请求发送给服务器的其他资源，资源可以是静态资源（如HTML文件）也可以动态资源(如 Servlet 或JSP 文件)。

  获取RequestDispatcher:

  ```java
  request.getRequestDispatcher(“**xxx**”)
  Copy
  ```

  转发资源：

  ```java
  void forward(ServletRequest request, ServletResponse response)
  Copy
  ```

  转发过程中，请求间的数据可以共享，可以进行数据传递。

  属性传递：

  - request.setAttribute("**key**", value);
  - request.getAttribute(“key”)
  - request.removeAttribute("key")

  **Servlet转发到JSP**

  EmployeeListServlet.java

  ```java
  request.setAttribute("list",list);
  request.getServletDispatcher("employee-list.jsp").forward(reqeust,response);Copy
  ```

```jsp
  <table>
    <c:forEach items="${list}" var="e">
          <tr>
            <td>${e.name}</td>
              <td>${e.age}</td>
          </tr>
      </c:forEach>  
  </table>Copy
```

- 重定向

  需要与客户端进行两次请求交互，可以使用响应对象调用sendRedirect(String location)方法，请求间数据不能共享，若想共享则可以使用会话实现。

  ```java
  response.sendRedirect("url");
  Copy
  ```

### 6.5 Session和Cookie

HTTP是一个无状态的通信协议，相当于每次客户端检索网页时，客户端打开一个单独的连接到 Web 服务器，服务器会自动不保留之前客户端请求的任何记录。为了追踪用户信息，可以采用以下两种通信方式。

***Cookie\***

Cookie是客户端技术，服务器把每个用户的数据以cookie的形式写在用户各自的浏览器中。客户端浏览器每次访问网站时，都会检测是否有该网站的 cookie 信息，这样，Web资源处理的就是用户各自的数据了。

***Session\***

Session是服务器端技术，利用这个技术，服务器在运行时可以为每一个用户的浏览器创建一个其独享的session对象，由于session为用户浏览器独享，所以用户在访问服务器的web资源时，可以把各自的数据放在各自的session中，当用户再去访问服务器中的其它web资源时，其它web资源再从用户各自的session中取出数据为用户服务。

***session\***应用

Servlet 提供了 HttpSession 接口，该接口提供了一种跨多个页面请求或访问网站时识别用户以及存储有关用户信息的方式，可以通过请求对象的getSession()方法获得HttpSession对象。

***HttpSession常用方法：\***

| ***方法\***                                  | ***说明\***                                               |
| -------------------------------------------- | --------------------------------------------------------- |
| void setAttribute(String name, Object value) | 使用指定的名称绑定一个对象到该 session 会话。             |
| Object getAttribute(String name)             | 返回指定名称的对象，如果没有指定名称的对象，则返回 null。 |
| void removeAttribute(String name)            | 将从该 session 会话移除指定名称的对象                     |
| void invalidate()                            | 设置session 会话无效，并解除绑定到它上面的任何对象        |
| String getId()                               | 返回session 会话的唯一标识符的字符串                      |

***Cookie应用\***

Cookie是Web服务器保存在用户硬盘上的一段文本，以名/值’对(name-value pairs)的形式储存。

Cookie的常用用途：

Cookie是站点跟踪特定访问者访问的次数，最后访问的时间以及访问者进入站点的路径；

能够帮助站点统计用户个人资料以实现各种各样的个性化服务。

可以实现自动登录功能，使得用户不需要输入用户名和密码就可以进入曾经浏览的站点。

***Cookie类的相关方法：\***

获得cookie对象可以使用构造方法：

public Cookie(String name,String value)

从请求中获取Cookie:

Cookie[] HttpServletRequest.getCookies()

向响应中添加cookie:

HttpServletResponse.addCookie(Cookie cookie)

***Cookie的常用方法：\***

| ***方法\***                | ***说明\***                              |
| -------------------------- | ---------------------------------------- |
| void setMaxAge(int expiry) | 该方法设置 cookie 过期的时间（以秒为单位 |
| int getMaxAge()            | 返回 cookie 的最大生存周期（以秒为单位） |
| String getName()           | 返回 cookie 的名称。名称在创建后不能改变 |