- 子类是不继承父类的构造函数的，它只是调用（隐式或显式）。
  如果父类的构造器带有参数，则必须在子类的构造器中显式地通过 **super 关键字调用父类的构造器**并配以适当的参数列表。
  如果父类构造器没有参数，则在子类的构造器中不需要使用 super 关键字调用父类构造器，系统会自动调用父类的无参构造器。
  ```java
  class Animal {
      String name;
      int age;
  
      public Animal(String name, int age) {
          this.name = name;
          this.age = age;
      }
  
      void eat() {
          System.out.println("animal : eat");
      }
  }
  class Cat extends Animal{
  
      public Cat(String name, int age) {
          super(name, age);
      }
      
  }
  ```
  
  **实例**
  ```java
   class Animal{
     public void move(){
        System.out.println("动物可以移动");
     }
  }
   
  class Dog extends Animal{
     public void move(){
        System.out.println("狗可以跑和走");
     }
  }
   
  public class TestDog{
     public static void main(String args[]){
        Animal a = new Animal(); // Animal 对象
        Animal b = new Dog(); // Dog 对象
   
        a.move();// 执行 Animal 类的方法
   
        b.move();//执行 Dog 类的方法
     }
  }
  ```
  在上面的例子中可以看到，尽管 b 属于 Animal 类型，但是它运行的是 Dog 类的 move方法。
  这是由于在编译阶段，只是检查参数的引用类型。
  然而在运行时，Java 虚拟机(JVM)指定对象的类型并且运行该对象的方法。
  因此在上面的例子中，之所以能编译成功，是因为 Animal 类中存在 move 方法，然而运行时，运行的是特定对象的方法。
  思考以下例子：
  ```java
  class Animal{
     public void move(){
        System.out.println("动物可以移动");
     }
  }
   
  class Dog extends Animal{
     public void move(){
        System.out.println("狗可以跑和走");
     }
     public void bark(){
        System.out.println("狗可以吠叫");
     }
  }
   
  public class TestDog{
     public static void main(String args[]){
        Animal a = new Animal(); // Animal 对象
        Animal b = new Dog(); // Dog 对象
   
        a.move();// 执行 Animal 类的方法
        b.move();//执行 Dog 类的方法
        b.bark();
     }
  }
  ```
  
  > 该程序将抛出一个编译错误，因为b的引用类型Animal没有bark方法。
-
-