- Java语言当中提供了一个叫做instanceof的运算符。很多教科书上对这个运算符的介绍并不详细，只是简单的说这个运算符是用来**判断某个对象是不是属于某种类型**。我们现在就用一篇短文介绍一下instanceof的运算符的作用和注意事项。请看下面的代码片段
- instanceof运算符在判断的过程中，看的是**引用实际指向的对象** (而不是看引用自身的类型)是不是可以被认定为属于某种类型。
-
- instanceof用来测试一个对象是否为一个类的实例，用法为：
- > boolean result = obj instanceof Class
- 其中 obj 为一个对象，Class 表示一个类或者一个接口，**当 obj 为 Class 的对象，或者是其直接或间接子类，或者是其接口的实现类，结果result 都返回 true，**否则返回false。
-
- 1、obj 必须为引用类型，不能是基本类型
- 2、obj 为 null ，那么将返回 false。
-
- 3、obj 为 class 类的实例对象
  
  ```java
  Integer integer = new Integer(1);
  System.out.println(integer instanceof  Integer);//true
  ```
  
  这没什么好说的，最普遍的一种用法。
-
- 4、obj 为 class 接口的实现类
-
- 5、obj 为 class 类的直接或间接子类
-
- 6、问题
  前面我们说过编译器会检查 obj 是否能转换成右边的class类型，如果不能转换则直接报错，如果不能确定类型，则通过编译，具体看运行时定。
  看如下几个例子：
  ```java
  Person p1 = new Person();
  System.out.println(p1 instanceof String);//编译报错
  System.out.println(p1 instanceof List);//false
  System.out.println(p1 instanceof List<?>);//false
  System.out.println(p1 instanceof List<Person>);//编译报错
  ```
  按照我们上面的说法，这里就存在问题了，Person 的对象 p1 很明显不能转换为 String 对象，那么自然 Person 的对象 p1 instanceof String 不能通过编译，但为什么 p1 instanceof List 却能通过编译呢？而 instanceof List 又不能通过编译了？a
  简单来说就是：如果 obj 不为 null 并且 (class) obj 不抛 ClassCastException 异常则该表达式值为 true ，否则值为 false 。
  所以对于上面提出的问题就很好理解了，为什么 p1 instanceof String 编译报错，因为(String)p1 是不能通过编译的，而 (List)p1 可以通过编译。