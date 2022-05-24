## 使用Lombok

我们发现，在以往编写项目时，尤其是在类进行类内部成员字段封装时，需要编写大量的get/set方法，这不仅使得我们类定义中充满了get和set方法，同时如果字段名称发生改变，又要挨个进行修改，甚至当字段变得很多时，构造方法的编写会非常麻烦！

通过使用Lombok（小辣椒）就可以解决这样的问题！

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Finews.gtimg.com%2Fnewsapp_bt%2F0%2F14004711543%2F1000&refer=http%3A%2F%2Finews.gtimg.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1638080575&t=91a3937a42d14fe8129b3761bbdef82c)

我们来看看，使用原生方式和小辣椒方式编写类的区别，首先是传统方式：

```java
public class Student {
private Integer sid;
private String name;
private String sex;

public Student(Integer sid, String name, String sex) {
    this.sid = sid;
    this.name = name;
    this.sex = sex;
}

public Integer getSid() {             //长！
    return sid;
}

public void setSid(Integer sid) {     //到！
    this.sid = sid;
}

public String getName() {             //爆！
    return name;
}

public void setName(String name) {    //炸！
    this.name = name;
}

public String getSex() {
    return sex;
}

public void setSex(String sex) {
    this.sex = sex;
}
}
```

而使用Lombok之后：

```java
@Getter
@Setter
@AllArgsConstructor
public class Student {
private Integer sid;
private String name;
private String sex;
}
```

我们发现，使用Lombok之后，只需要添加几个注解，就能够解决掉我们之前长长的一串代码！
### 配置Lombok
-
-