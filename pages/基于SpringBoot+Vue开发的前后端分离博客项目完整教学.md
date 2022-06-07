## P1 1、项目简介与大纲
### 项目演示：
[www.markerhub.com:8084/blogs](https://link.juejin.cn/?target=http%3A%2F%2Fwww.markerhub.com%3A8084%2Fblogs "http://www.markerhub.com:8084/blogs")
### 效果：
![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/6e25b3a7df92467ea08888f72d28a205.png)
### 资料：
vueblog项目文档： [超详细！4小时开发一个SpringBoot+vue前后端分离博客项目！！](https://www.zhuawaba.com/post/17) #文章

vueblog项目视频： https://www.bilibili.com/video/BV1PQ4y1P7hZ

vueblog代码仓库： https://github.com/MarkerHub/vueblog   #github

SQL脚本： https://github.com/markerhub/vueblog/blob/master/vueblog-java/src/main/resources/vueblog.sql

加群学习：请添加微信 java-mindman3，备注【VueBlog】我会拉你进群！
### 我的前言：
不错的项目，得做出来，慢慢做，笔记多做点。
### 我的感悟：
2022-06-07 周二 21:28:43 
刚看完P2，
大佬的这个笔记 [超详细！4小时开发一个SpringBoot+vue前后端分离博客项目！！](https://www.zhuawaba.com/post/17) 真详细，
这就是做笔记的学习模板啊！我就是拖狗屎！学习学习学习！
## P2   2、新建SpringBoot项目，并整合mybatis plus
### 新建Springboot项目
![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1654605741869.png)


这里，我们使用IDEA来开发我们项目，新建步骤比较简单，我们就不截图了。
开发工具与环境：
*   idea
*   mysql
*   jdk 8
*   maven3.3.9

*   devtools：项目的热加载重启插件
*   lombok：简化代码的工具
*   Web
*   mysql

新建好的项目结构如下，SpringBoot版本使用的目前最新的2.2.6.RELEASE版本

![图片](https://image-1300566513.cos.ap-guangzhou.myqcloud.com/upload/images/20201117/be1226b48ef24b2bbe00d75996bd7f89.png)
### 整合mybatis plus

接下来，我们来整合mybatis plus，让项目能完成基本的增删改查操作。步骤很简单：可以去官网看看：[https://mp.baomidou.com/guide/install.html](https://mp.baomidou.com/guide/install.html)

**第一步：导入jar包**

pom中导入mybatis plus的jar包，因为后面会涉及到代码生成，所以我们还需要导入页面模板引擎，这里我们用的是freemarker。

```
<!--mp-->
<dependency>
  <groupId>com.baomidou</groupId>
  <artifactId>mybatis-plus-boot-starter</artifactId>
  <version>3.2.0</version>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-freemarker</artifactId>
</dependency>
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <scope>runtime</scope>
</dependency>
<!--mp代码生成器-->
<dependency>
  <groupId>com.baomidou</groupId>
  <artifactId>mybatis-plus-generator</artifactId>
  <version>3.2.0</version>
</dependency>
```

**第二步：然后去写配置文件**
application.yml

```
# DataSource Config
spring:
datasource:
  driver-class-name: com.mysql.cj.jdbc.Driver
  url: jdbc:mysql://localhost:3306/vueblog?useUnicode=true&useSSL=false&characterEncoding=utf8&serverTimezone=Asia/Shanghai
  username: root
  password: admin
mybatis-plus:
mapper-locations: classpath*:/mapper/**Mapper.xml
```

上面除了配置数据库的信息，还配置了myabtis plus的mapper的xml文件的扫描路径，这一步不要忘记了。(并且要注意 **数据库的配置填自己的**)

**第三步：开启mapper接口扫描，添加分页插件**

新建一个包：通过[@mapperScan](https://github.com/mapperScan "@mapperScan")注解指定要变成实现类的接口所在的包，然后包下面的所有接口在编译之后都会生成相应的实现类。PaginationInterceptor是一个分页插件。

*   com.markerhub.config.MybatisPlusConfig

```java
@Configuration
@EnableTransactionManagement
@MapperScan("com.markerhub.mapper")
public class MybatisPlusConfig {
  @Bean
  public PaginationInterceptor paginationInterceptor() {
      PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
      return paginationInterceptor;
  }
}
```

**第四步：代码生成**

如果你没再用其他插件，那么现在就已经可以使用mybatis plus了，官方给我们提供了一个代码生成器，然后我写上自己的参数之后，就可以直接根据数据库表信息生成entity、service、mapper等接口和实现类。

*   com.markerhub.CodeGenerator

因为代码比较长，就不贴出来了，在代码仓库上看哈！
我得写出来，注意关于数据库的配置
![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1654609137928.png)

```java
package com.markerhub;  

import com.baomidou.mybatisplus.core.exceptions.MybatisPlusException;  
import com.baomidou.mybatisplus.core.toolkit.StringPool;  
import com.baomidou.mybatisplus.core.toolkit.StringUtils;  
import com.baomidou.mybatisplus.generator.AutoGenerator;  
import com.baomidou.mybatisplus.generator.InjectionConfig;  
import com.baomidou.mybatisplus.generator.config.*;  
import com.baomidou.mybatisplus.generator.config.po.TableInfo;  
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;  
import com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine;  

import java.util.ArrayList;  
import java.util.List;  
import java.util.Scanner;  

// 演示例子，执行 main 方法控制台输入模块表名回车自动生成对应项目目录中  
public class CodeGenerator {  

  /**  
   * <p>  
   * 读取控制台内容  
   * </p>  
   */  
  public static String scanner(String tip) {  
      Scanner scanner = new Scanner(System.in);  
      StringBuilder help = new StringBuilder();  
      help.append("请输入" + tip + "：");  
      System.out.println(help.toString());  
      if (scanner.hasNext()) {  
          String ipt = scanner.next();  
          if (StringUtils.isNotEmpty(ipt)) {  
              return ipt;  
          }  
      }  
      throw new MybatisPlusException("请输入正确的" + tip + "！");  
  }  

  public static void main(String[] args) {  
      // 代码生成器  
      AutoGenerator mpg = new AutoGenerator();  

      // 全局配置  
      GlobalConfig gc = new GlobalConfig();  
      String projectPath = System.getProperty("user.dir");  
      gc.setOutputDir(projectPath + "/src/main/java");  
//        gc.setOutputDir("D:\\test");  
      gc.setAuthor("关注公众号：MarkerHub");  
      gc.setOpen(false);  
      // gc.setSwagger2(true); 实体属性 Swagger2 注解  
      gc.setServiceName("%sService");  
      mpg.setGlobalConfig(gc);  

      // 数据源配置  
      DataSourceConfig dsc = new DataSourceConfig();  
      dsc.setUrl("jdbc:mysql://localhost:3306/vueblog?useUnicode=true&useSSL=false&characterEncoding=utf8&serverTimezone=UTC");  
      // dsc.setSchemaName("public");  
      dsc.setDriverName("com.mysql.cj.jdbc.Driver");  
      dsc.setUsername("root");  
      dsc.setPassword("123456");  
      mpg.setDataSource(dsc);  

      // 包配置  
      PackageConfig pc = new PackageConfig();  
      pc.setModuleName(null);  
      pc.setParent("com.markerhub");  
      mpg.setPackageInfo(pc);  

      // 自定义配置  
      InjectionConfig cfg = new InjectionConfig() {  
          @Override  
          public void initMap() {  
              // to do nothing  
          }  
      };  

      // 如果模板引擎是 freemarker        String templatePath = "/templates/mapper.xml.ftl";  
      // 如果模板引擎是 velocity        // String templatePath = "/templates/mapper.xml.vm";  
      // 自定义输出配置  
      List<FileOutConfig> focList = new ArrayList<>();  
      // 自定义配置会被优先输出  
      focList.add(new FileOutConfig(templatePath) {  
          @Override  
          public String outputFile(TableInfo tableInfo) {  
              // 自定义输出文件名 ， 如果你 Entity 设置了前后缀、此处注意 xml 的名称会跟着发生变化！！  
              return projectPath + "/src/main/resources/mapper/"  
                      + "/" + tableInfo.getEntityName() + "Mapper" + StringPool.DOT_XML;  
          }  
      });  

      cfg.setFileOutConfigList(focList);  
      mpg.setCfg(cfg);  

      // 配置模板  
      TemplateConfig templateConfig = new TemplateConfig();  

      templateConfig.setXml(null);  
      mpg.setTemplate(templateConfig);  

      // 策略配置  
      StrategyConfig strategy = new StrategyConfig();  
      strategy.setNaming(NamingStrategy.underline_to_camel);  
      strategy.setColumnNaming(NamingStrategy.underline_to_camel);  
      strategy.setEntityLombokModel(true);  
      strategy.setRestControllerStyle(true);  
      strategy.setInclude(scanner("表名，多个英文逗号分割").split(","));  
      strategy.setControllerMappingHyphenStyle(true);  
      strategy.setTablePrefix("m_");  
      mpg.setStrategy(strategy);  
      mpg.setTemplateEngine(new FreemarkerTemplateEngine());  
      mpg.execute();  
  }  
}
```

首先我在数据库中新建了一个user表：

```
CREATE TABLE `m_user` (
`id` bigint(20) NOT NULL AUTO_INCREMENT,
`username` varchar(64) DEFAULT NULL,
`avatar` varchar(255) DEFAULT NULL,
`email` varchar(64) DEFAULT NULL,
`password` varchar(64) DEFAULT NULL,
`status` int(5) NOT NULL,
`created` datetime DEFAULT NULL,
`last_login` datetime DEFAULT NULL,
PRIMARY KEY (`id`),
KEY `UK_USERNAME` (`username`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
CREATE TABLE `m_blog` (
`id` bigint(20) NOT NULL AUTO_INCREMENT,
`user_id` bigint(20) NOT NULL,
`title` varchar(255) NOT NULL,
`description` varchar(255) NOT NULL,
`content` longtext,
`created` datetime NOT NULL ON UPDATE CURRENT_TIMESTAMP,
`status` tinyint(4) DEFAULT NULL,
PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=11 DEFAULT CHARSET=utf8mb4;
INSERT INTO `vueblog`.`m_user` (`id`, `username`, `avatar`, `email`, `password`, `status`, `created`, `last_login`) VALUES ('1', 'markerhub', 'https://image-1300566513.cos.ap-guangzhou.myqcloud.com/upload/images/5a9f48118166308daba8b6da7e466aab.jpg', NULL, '96e79218965eb72c92a549dd5a330112', '0', '2020-04-20 10:44:01', NULL);
```

运行CodeGenerator的main方法，输入表名：m_user，

![图片](https://image-1300566513.cos.ap-guangzhou.myqcloud.com/upload/images/20201117/47682f628eae4c3588f6729e1df322af.png)

生成结果如下：得到：(这个真牛！直接帮我们生成了)

![图片](https://image-1300566513.cos.ap-guangzhou.myqcloud.com/upload/images/20201117/f5e4daec518044619c0d374daa844f40.png)

简洁！方便！经过上面的步骤，基本上我们已经把mybatis plus框架集成到项目中了。

ps：额，注意一下m_blog表的代码也生成一下哈。

在UserController中写个测试：

```
@RestController
@RequestMapping("/user")
public class UserController {
  @Autowired
  UserService userService;
  @GetMapping("/{id}")
  public Object test(@PathVariable("id") Long id) {
      return userService.getById(id);
  }
}
```

访问：[http://localhost:8080/user/1](http://localhost:8080/user/1) 获得结果如下，整合成功！
![图片](https://image-1300566513.cos.ap-guangzhou.myqcloud.com/upload/images/20201117/ecb8c3a623694595a22980a26821ccb6.png)


mybatis-plus真尼玛强，直接根据一个CodeGenerator.java类生成 项目主题结构
![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1654608118024.png)




P3 3-1、统一结果封装
P4 4-1、Shiro整合jwt逻辑分析
P5 4-2、shiro编码整合
P6 4-3、修正上节课遗留的点问题
P7 4-4、shiro登录逻辑开发
P8 5、全局异常处理
P9 6、实体校验处理
P10 7、解决跨域问题