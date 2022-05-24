- 创建对象
  对象是根据类创建的。在Java中，使用关键字 new 来创建一个新的对象。创建对象需要以下三步：
  
  声明：声明一个对象，包括对象类型和对象名称。
  实例化：使用关键字 new 来创建一个对象。
  初始化：使用 new 创建对象时，会调用构造方法初始化对象。
  
  下面是一个创建对象的例子：
  ```java
  public class Puppy{
     public Puppy(String name){
        //这个构造器仅有一个参数：name
        System.out.println("小狗的名字是 : " + name ); 
     }
     public static void main(String[] args){
        // 下面的语句将创建一个Puppy对象
        Puppy myPuppy = new Puppy( "tommy" );
     }
  }
  ```
  编译并运行上面的程序，会打印出下面的结果：
  ```java
  小狗的名字是 : tommy
  ```
- **访问成员变量和方法**
  通过已**创建的对象**来访问成员变量和成员方法，如下所示：
  ```java
  /* 实例化对象 */
  Object referenceVariable = new Constructor();
  /* 访问类中的变量 */
  referenceVariable.variableName;
  /* 访问类中的方法 */
  referenceVariable.methodName();
  ```
  实例
  下面的例子展示如何访问实例变量和调用成员方法：
  **程序都是从main方法开始执行。为了能运行这个程序，必须包含main方法并且创建一个实例对象**。
  ```java
  public class Test {
      String name;
      int age;
      String status;
  
      public Test(String name, int age, String status) {
          this.name = name;
          this.age = age;
          this.status = status;
      }
  
      void print(){
          System.out.println(name);
          System.out.println(age);
          System.out.println(status);
      }
    
    public static void main(String[] args) {
          Test test = new Test("王冠荆棘",20,"学习Java");
          test.print();
      }
  }
  ```
  编译并运行上面的程序，产生如下结果：
  ``` 
  王冠荆棘
  20
  学习Java
  ```
-