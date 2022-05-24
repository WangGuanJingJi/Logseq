- 在Java语言中，所有的变量在使用前必须声明。声明变量的基本格式如下：
- ``` 
  type identifier [ = value][, identifier [= value] ...] ;
  ```
  格式说明：type为Java数据类型。identifier是变量名。可以使用逗号隔开来声明多个同类型变量。
- 以下列出了一些变量的声明实例。注意有些包含了初始化过程。
  ```java
  int a, b, c;         // 声明三个int型整数：a、 b、c
  int d = 3, e = 4, f = 5; // 声明三个整数并赋予初值
  byte z = 22;         // 声明并初始化 z
  String s = "runoob";  // 声明并初始化字符串 s
  double pi = 3.14159; // 声明了双精度浮点型变量 pi
  char x = 'x';        // 声明变量 x 的值是字符 'x'。
  ```
- Java语言支持的变量类型有：
  实例变量：独立于方法之外的变量，不过没有 static 修饰。
  局部变量：类的方法中的变量。
  类变量：独立于方法之外的变量，用 static 修饰。
  ```java
  public class Variable{
      String str="hello world";  // 实例变量
      static int allClicks=0;    // 类变量
      public void method(){
   
          int i =0;  // 局部变量
   
      }
  }
  ```
-
- ## Java 局部变量
  局部变量声明在方法、构造方法或者语句块中；
  局部变量在方法、构造方法、或者语句块被执行的时候创建，当它们执行完成后，变量将会被销毁；
  **访问修饰符不能用于局部变量；**
  局部变量只在声明它的方法、构造方法或者语句块中可见；
  局部变量是在栈上分配的。
  **局部变量没有默认值，所以局部变量被声明后，必须经过初始化，才可以使用**
-
- ## 实例变量
  实例变量声明在一个类中，但在方法、构造方法和语句块之外；
  当一个对象被实例化之后，每个实例变量的值就跟着确定；
  实例变量在对象创建的时候创建，在对象被销毁的时候销毁；
  实例变量的值应该至少被一个方法、构造方法或者语句块引用，使得外部能够通过这些方式获取实例变量信息；
  **实例变量可以声明在使用前或者使用后；**
  访问修饰符可以修饰实例变量；
  实例变量对于类中的方法、构造方法或者语句块是可见的。**一般情况下应该把实例变量设为私有**。
  实例变量具有默认值。数值型变量的默认值是0，布尔型变量的默认值是false，引用类型变量的默认值是null。**变量的值可以在声明时指定，也可以在构造方法中指定；**
  ```java
  import java.io.*;
  public class Employee{
     // 这个实例变量对子类可见
     public String name;
     // 私有变量，仅在该类可见
     private double salary;
     //在构造器中对name赋值
     public Employee (String empName){
        name = empName;
     }
     //方法，设定salary的值
     public void setSalary(double empSal){
        salary = empSal;
     }  
     // 打印信息
     public void printEmp(){
        System.out.println("名字 : " + name );
        System.out.println("薪水 : " + salary);
     }
   
     public static void main(String[] args){
        Employee empOne = new Employee("RUNOOB");
        empOne.setSalary(1000.0);
        empOne.printEmp();
     }
  }
  ```
-
- ## 类变量（静态变量）
  类变量被声明为 public static final 类型时，类变量名称一般建议使用大写字母
-
-