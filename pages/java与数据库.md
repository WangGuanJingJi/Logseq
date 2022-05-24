- 通过Java如何去使用数据库来帮助我们存储数据呢，这将是本章节讨论的重点。
## 初识JDBC

JDBC是什么？JDBC英文名为：Java Data Base Connectivity(Java数据库连接)，官方解释它是Java编程语言和广泛的数据库之间独立于数据库的连接标准的Java API，根本上说JDBC是一种规范，它提供的接口，一套完整的，允许便捷式访问底层数据库。可以用JAVA来写不同类型的可执行文件：JAVA应用程序、JAVA Applets、Java Servlet、JSP等，不同的可执行文件都能通过JDBC访问数据库，又兼备存储的优势。简单说它就是Java与数据库的连接的桥梁或者插件，用Java代码就能操作数据库的增删改查、存储过程、事务等。

我们可以发现，JDK自带了一个`java.sql`包，而这里面就定义了大量的接口，不同类型的数据库，都可以通过实现此接口，编写适用于自己数据库的实现类。而不同的数据库厂商实现的这套标准，我们称为`数据库驱动`。
### 准备工作

那么我们首先来进行一些准备工作，以便开始JDBC的学习：

* 将idea连接到我们的数据库，以便以后调试。
* 将mysql驱动jar依赖导入到项目中（推荐6.0版本以上，这里用到是8.0）
* 向Jetbrians申请一个学生/教师授权，用于激活idea终极版（进行JavaWeb开发需要用到，一般申请需要3-7天时间审核）不是大学生的话...emmm...懂的都懂。
* 教育授权申请地址：https://www.jetbrains.com/shop/eform/students

一个Java程序并不是一个人的战斗，我们可以在别人开发的基础上继续向上开发，其他的开发者可以将自己编写的Java代码打包为`jar`，我们只需要导入这个`jar`作为依赖，即可直接使用别人的代码，就像我们直接去使用JDK提供的类一样。
### 使用JDBC连接数据库

**注意：**6.0版本以上，不用手动加载驱动，我们直接使用即可！

```java
//1. 通过DriverManager来获得数据库连接
try (Connection connection = DriverManager.getConnection("连接URL","用户名","密码");
   //2. 创建一个用于执行SQL的Statement对象
   Statement statement = connection.createStatement()){   //注意前两步都放在try()中，因为在最后需要释放资源！
  //3. 执行SQL语句，并得到结果集
  ResultSet set = statement.executeQuery("select * from 表名");
  //4. 查看结果
  while (set.next()){
      ...
  }
}catch (SQLException e){
  e.printStackTrace();
}
//5. 释放资源，try-with-resource语法会自动帮助我们close
```

其中，连接的URL如果记不住格式，我们可以打开idea的数据库连接配置，复制一份即可。（其实idea本质也是使用的JDBC，整个idea程序都是由Java编写的，实际上idea就是一个Java程序）
### 了解DriverManager

我们首先来了解一下DriverManager是什么东西，它其实就是管理我们的数据库驱动的：

```java
public static synchronized void registerDriver(java.sql.Driver driver,
      DriverAction da)
  throws SQLException {

  /* Register the driver if it has not already been added to our list */
  if(driver != null) {
      registeredDrivers.addIfAbsent(new DriverInfo(driver, da));    //在刚启动时，mysql实现的驱动会被加载，我们可以断点调试一下。
  } else {
      // This is for compatibility with the original DriverManager
      throw new NullPointerException();
  }

  println("registerDriver: " + driver);

}
```

我们可以通过调用getConnection()来进行数据库的链接：

```java
@CallerSensitive
public static Connection getConnection(String url,
  String user, String password) throws SQLException {
  java.util.Properties info = new java.util.Properties();

  if (user != null) {
      info.put("user", user);
  }
  if (password != null) {
      info.put("password", password);
  }

  return (getConnection(url, info, Reflection.getCallerClass()));   //内部有实现
}
```

我们可以手动为驱动管理器添加一个日志打印：

```java
static {
  DriverManager.setLogWriter(new PrintWriter(System.out));   //这里直接设定为控制台输出
}
```

现在我们执行的数据库操作日志会在控制台实时打印。
### 了解Connection

Connection是数据库的连接对象，可以通过连接对象来创建一个Statement用于执行SQL语句：

```java
Statement createStatement() throws SQLException;
```

我们发现除了普通的Statement，还存在PreparedStatement：

```java
PreparedStatement prepareStatement(String sql)
  throws SQLException;
```

在后面我们会详细介绍PreparedStatement的使用，它能够有效地预防SQL注入式攻击。

它还支持事务的处理，也放到后面来详细进行讲解。
### 了解Statement

我们发现，我们之前使用了`executeQuery()`方法来执行`select`语句，此方法返回给我们一个ResultSet对象，查询得到的数据，就存放在ResultSet中！

Statement除了执行这样的DQL语句外，我们还可以使用`executeUpdate()`方法来执行一个DML或是DDL语句，它会返回一个int类型，表示执行后受影响的行数，可以通过它来判断DML语句是否执行成功。

也可以通过`excute()`来执行任意的SQL语句，它会返回一个`boolean`来表示执行结果是一个ResultSet还是一个int，我们可以通过使用`getResultSet()`或是`getUpdateCount()`来获取。
### 执行DML操作

我们通过几个例子来向数据库中插入数据。
### 执行DQL操作

执行DQL操作会返回一个ResultSet对象，我们来看看如何从ResultSet中去获取数据：

```java
//首先要明确，select返回的数据类似于一个excel表格
while (set.next()){
  //每调用一次next()就会向下移动一行，首次调用会移动到第一行
}
```

我们在移动行数后，就可以通过set中提供的方法，来获取每一列的数据。

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg-blog.csdnimg.cn%2F202005062358238.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1JlZ2lubw%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70&refer=http%3A%2F%2Fimg-blog.csdnimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1638091193&t=bf37a5cb988d0a641d00c7e325d06ce7)
### 执行批处理操作

当我们要执行很多条语句时，可以不用一次一次地提交，而是一口气全部交给数据库处理，这样会节省很多的时间。

```java
public static void main(String[] args) throws ClassNotFoundException {
  try (Connection connection = DriverManager.getConnection();
       Statement statement = connection.createStatement()){

      statement.addBatch("insert into user values ('f', 1234)");
      statement.addBatch("insert into user values ('e', 1234)");   //添加每一条批处理语句
      statement.executeBatch();   //一起执行

  }catch (SQLException e){
      e.printStackTrace();
  }
}
```
### 将查询结果映射为对象
- 既然我们现在可以从数据库中获取数据了，那么现在就可以将这些数据转换为一个类来进行操作，首先定义我们的实体类：
  
  ```java
  public class Student {
      Integer sid;
      String name;
      String sex;
  
      public Student(Integer sid, String name, String sex) {
          this.sid = sid;
          this.name = name;
          this.sex = sex;
      }
  
      public void say(){
          System.out.println("我叫："+name+"，学号为："+sid+"，我的性别是："+sex);
      }
  }
  ```
  
  现在我们来进行一个转换：
  
  ```java
  while (set.next()){
      Student student = new Student(set.getInt(1), set.getString(2), set.getString(3));
      student.say();
  }
  ```
  
  **注意：**列的下标是从1开始的。
  
  我们也可以利用反射机制来将查询结果映射为对象，使用反射的好处是，无论什么类型都可以通过我们的方法来进行实体类型映射：
  
  ```java
  private static <T> T convert(ResultSet set, Class<T> clazz){
      try {
          Constructor<T> constructor = clazz.getConstructor(clazz.getConstructors()[0].getParameterTypes());   //默认获取第一个构造方法
          Class<?>[] param = constructor.getParameterTypes();  //获取参数列表
          Object[] object = new Object[param.length];  //存放参数
          for (int i = 0; i < param.length; i++) {   //是从1开始的
              object[i] = set.getObject(i+1);
              if(object[i].getClass() != param[i])
                  throw new SQLException("错误的类型转换："+object[i].getClass()+" -> "+param[i]);
          }
          return constructor.newInstance(object);
      } catch (ReflectiveOperationException | SQLException e) {
          e.printStackTrace();
          return null;
      }
  }
  ```
  
  现在我们就可以通过我们的方法来将查询结果转换为一个对象了：
  
  ```java
  while (set.next()){
      Student student = convert(set, Student.class);
      if(student != null) student.say();
  }
  ```
  
  实际上，在后面我们会学习Mybatis框架，它对JDBC进行了深层次的封装，而它就进行类似上面反射的操作来便于我们对数据库数据与实体类的转换。
### 实现登陆与SQL注入攻击

在使用之前，我们先来看看如果我们想模拟登陆一个用户，我们该怎么去写：

```java
try (Connection connection = DriverManager.getConnection("URL","用户名","密码");
   Statement statement = connection.createStatement();
   Scanner scanner = new Scanner(System.in)){
  ResultSet res = statement.executeQuery("select * from user where username='"+scanner.nextLine()+"'and pwd='"+scanner.nextLine()+"';");
  while (res.next()){
      String username = res.getString(1);
      System.out.println(username+" 登陆成功！");
  }
}catch (SQLException e){
  e.printStackTrace();
}
```

用户可以通过自己输入用户名和密码来登陆，乍一看好像没啥问题，那如果我输入的是以下内容呢：

```sql
Test
1111' or 1=1; -- 
# Test 登陆成功！
```

1=1一定是true，那么我们原本的SQL语句会变为：

```sql
select * from user where username='Test' and pwd='1111' or 1=1; -- '
```

我们发现，如果允许这样的数据插入，那么我们原有的SQL语句结构就遭到了破坏，使得用户能够随意登陆别人的账号。因此我们可能需要限制用户的输入来防止用户输入一些SQL语句关键字，但是关键字非常多，这并不是解决问题的最好办法。
### 使用PreparedStatement

我们发现，如果单纯地使用Statement来执行SQL命令，会存在严重的SQL注入攻击漏洞！而这种问题，我们可以使用PreparedStatement来解决：

```java
public static void main(String[] args) throws ClassNotFoundException {
  try (Connection connection = DriverManager.getConnection("URL","用户名","密码");
       PreparedStatement statement = connection.prepareStatement("select * from user where username= ? and pwd=?;");
       Scanner scanner = new Scanner(System.in)){

      statement.setString(1, scanner.nextLine());
      statement.setString(2, scanner.nextLine());
      System.out.println(statement);    //打印查看一下最终执行的
      ResultSet res = statement.executeQuery();
      while (res.next()){
          String username = res.getString(1);
          System.out.println(username+" 登陆成功！");
      }
  }catch (SQLException e){
      e.printStackTrace();
  }
}
```

我们发现，我们需要提前给到PreparedStatement一个SQL语句，并且使用`?`作为占位符，它会预编译一个SQL语句，通过直接将我们的内容进行替换的方式来填写数据。使用这种方式，我们之前的例子就失效了！我们来看看实际执行的SQL语句是什么：

```
com.mysql.cj.jdbc.ClientPreparedStatement: select * from user where username= 'Test' and pwd='123456'' or 1=1; -- ';
```

我们发现，我们输入的参数一旦出现`'`时，会被变为转义形式`\'`，而最外层有一个真正的`'`来将我们输入的内容进行包裹，因此它能够有效地防止SQL注入攻击！
-
### 管理事务

JDBC默认的事务处理行为是自动提交，所以前面我们执行一个SQL语句就会被直接提交（相当于没有启动事务），所以JDBC需要进行事务管理时，首先要通过Connection对象调用setAutoCommit(false) 方法, 将SQL语句的提交（commit）由驱动程序转交给应用程序负责。

```java
con.setAutoCommit();   //关闭自动提交后相当于开启事务。
// SQL语句
// SQL语句
// SQL语句
con.commit();或 con.rollback();
```

一旦关闭自动提交，那么现在执行所有的操作如果在最后不进行`commit()`来提交事务的话，那么所有的操作都会丢失，只有提交之后，所有的操作才会被保存！也可以使用`rollback()`来手动回滚之前的全部操作！

```java
public static void main(String[] args) throws ClassNotFoundException {
  try (Connection connection = DriverManager.getConnection("URL","用户名","密码");
       Statement statement = connection.createStatement()){

      connection.setAutoCommit(false);  //关闭自动提交，现在将变为我们手动提交
      statement.executeUpdate("insert into user values ('a', 1234)");
      statement.executeUpdate("insert into user values ('b', 1234)");
      statement.executeUpdate("insert into user values ('c', 1234)");

      connection.commit();   //如果前面任何操作出现异常，将不会执行commit()，之前的操作也就不会生效
  }catch (SQLException e){
      e.printStackTrace();
  }
}
```

我们来接着尝试一下使用回滚操作：

```java
public static void main(String[] args) throws ClassNotFoundException {
  try (Connection connection = DriverManager.getConnection("URL","用户名","密码");
       Statement statement = connection.createStatement()){

      connection.setAutoCommit(false);  //关闭自动提交，现在将变为我们手动提交
      statement.executeUpdate("insert into user values ('a', 1234)");
      statement.executeUpdate("insert into user values ('b', 1234)");

      connection.rollback();   //回滚，撤销前面全部操作

      statement.executeUpdate("insert into user values ('c', 1234)");

      connection.commit();   //提交事务（注意，回滚之前的内容都没了）

  }catch (SQLException e){
      e.printStackTrace();
  }
}
```

同样的，我们也可以去创建一个回滚点来实现定点回滚：

```java
public static void main(String[] args) throws ClassNotFoundException {
  try (Connection connection = DriverManager.getConnection("URL","用户名","密码");
       Statement statement = connection.createStatement()){

      connection.setAutoCommit(false);  //关闭自动提交，现在将变为我们手动提交
      statement.executeUpdate("insert into user values ('a', 1234)");
      
      Savepoint savepoint = connection.setSavepoint();   //创建回滚点
      statement.executeUpdate("insert into user values ('b', 1234)");

      connection.rollback(savepoint);   //回滚到回滚点，撤销前面全部操作

      statement.executeUpdate("insert into user values ('c', 1234)");

      connection.commit();   //提交事务（注意，回滚之前的内容都没了）

  }catch (SQLException e){
      e.printStackTrace();
  }
}
```

通过开启事务，我们就可以更加谨慎地进行一些操作了，如果我们想从事务模式切换为原有的自动提交模式，我们可以直接将其设置回去：

```java
public static void main(String[] args) throws ClassNotFoundException {
  try (Connection connection = DriverManager.getConnection("URL","用户名","密码");
       Statement statement = connection.createStatement()){

      connection.setAutoCommit(false);  //关闭自动提交，现在将变为我们手动提交
      statement.executeUpdate("insert into user values ('a', 1234)");
      connection.setAutoCommit(true);   //重新开启自动提交，开启时把之前的事务模式下的内容给提交了
      statement.executeUpdate("insert into user values ('d', 1234)");
      //没有commit也成功了！
  }catch (SQLException e){
      e.printStackTrace();
  }

```

通过学习JDBC，我们现在就可以通过Java来访问和操作我们的数据库了！为了更好地衔接，我们还会接着讲解主流持久层框架——Mybatis，加深JDBC的记忆。
-