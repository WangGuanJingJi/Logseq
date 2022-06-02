-
- 介绍
	- 瑞吉外卖是专门为餐饮企业(餐厅、饭店)定制的一款软件产品。该项目的业务架构包含
	  后台管理系统+移动端前台(H5+小程序也全面覆盖)。
	- 在技术架构上，基于主流框架的 SpringBoot+Mybatis plus，打造用户层、网管层、应用层、数据层。学完该项目即可实现该类型项目的快速开发。
	- 黑马《瑞吉外卖》项目包含 14 天课程，共 190 节，涉及外卖业务开发6天、Git、Linux、Redis与项目优化 5 大篇章，涵盖整个项目的开发过程和优化过程。(那我也必须在6号搞出来瑞吉外卖项目！后面考虑学习)
	-
	- 极力推荐，有一定Java基础，想要快速了解项目开发流程，增长开发经验的同学们都快来学习噢!
-
-
- 课程详情：
	- 第一部分：瑞吉外卖项目
	- 1-本章内容介绍.
	- 2-软件开发整体介绍.
	- 3-瑞吉外卖项目整体介绍.
	- 4-开发环境搭建_数据库环境搭建.
	- 5-开发环境搭建_Maven项目搭建.
		- 配置pom.xml
		- 配置application.yml
		- SpringBoot 项目启动类
			- ``` java
			  			  @SpringBootApplication
			  			  public class ReggieApplication {
			  			      public static void main(String[] args) {
			  			          SpringApplication.run(ReggieApplication.class,args);
			  			          log.info("项目启动成功...");
			  			      }
			  			  }
			  ```
		- config文件夹 配置类
			- 配置WebMvcConfig -> 配置静态资源路径
				- ``` java
				  				  @Configuration
				  				  public class WebMvcConfig extends WebMvcConfigurationSupport {
				  				      @Override
				  				      protected void addResourceHandlers(ResourceHandlerRegistry registry) {
				  				          registry.addResourceHandler("/backend/**").addResourceLocations("classpath:/backend/");
				  				          registry.addResourceHandler("/front/**").addResourceLocations("classpath:/front/");
				  				      }
				  				  }
				  ```
				-
		-
	- 6-后台系统登录功能_需求分析.
		- login.html
			- 页面用Vue写的(所有的前端资源直接导入视频提供资源，现在不用自己写，但要能看懂)
			- 小细节，登录按钮左边一个小圆圈 转圈
				- ![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1654155175448.png)
		- 当用户点击登录后，会发送请求，到我们的服务端一些组件，先请求controller,controller再调service,service调mapper,最终与数据库进行交互。
			- ![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1654154931452.png)
	- 7-后台系统登录功能_代码开发(创建Controller、Service、Mapper、实体类).
		- 使用Mybatis-plus ，我没学，算了，直接跟着敲，了解下项目大概过程
	- 8-后台系统登录功能_代码开发(导入通用返回结果类).
		- ``` java
		  		  package com.itheima.reggie.common;
		  		  
		  		  import lombok.Data;
		  		  import java.util.HashMap;
		  		  import java.util.Map;
		  		  
		  		  @Data
		  		  public class R<T> {
		  		  
		  		      private Integer code; //编码：1成功，0和其它数字为失败
		  		  
		  		      private String msg; //错误信息
		  		  
		  		      private T data; //数据
		  		  
		  		      private Map map = new HashMap(); //动态数据
		  		  
		  		      public static <T> R<T> success(T object) {
		  		          R<T> r = new R<T>();
		  		          r.data = object;
		  		          r.code = 1;
		  		          return r;
		  		      }
		  		  
		  		      public static <T> R<T> error(String msg) {
		  		          R r = new R();
		  		          r.msg = msg;
		  		          r.code = 0;
		  		          return r;
		  		      }
		  		  
		  		      public R<T> add(String key, Object value) {
		  		          this.map.put(key, value);
		  		          return this;
		  		      }
		  		  
		  		  }
		  		  
		  ```
		-
	-
	- 9-后台系统登录功能_代码开发(梳理登录方法处理逻辑).
		- ![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1654160563091.png)
	-
	- 10-后台系统登录功能_代码开发(实现登录处理逻辑).
		- **写代码的时候，先去分析业务逻辑，步骤，然后再进行敲代码。(那我不就可以根据逻辑百度了)**
		- 听蒙了！完全不会！
	-
	- 11-后台系统登录功能_功能测试.
		- 磕磕绊绊，总算成功
	-
	- 12-**后台系统退出功能**_需求分析&代码开发&功能测试.
		- 直接过，不研究了，
	-
	- 13-分析后台系统首页构成和效果展示方式.
		- 还得学下Vue，去年11月学了一段时间，但都基本忘光了。
		- 可以魔改下 index.html ,实现我的视频收藏集 页面   #编程想法
			- ![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1654165757710.png)
		-
	-
	- (2022-06-02 周四 18:36:34 登录后台管理 这一章发现自己很多函数不知道，了解了下步骤，虽然不理解，但强行记下也好)
	-
	- **第二章**
	- 14-本章内容介绍.
		- ![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1654166308645.png)
		-
	-
	-
	- ### 15-**完善登录功能**_问题分析并创建过滤器.
		- 问题分析
			- ![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1654166583395.png)
		- 创建过滤器
			- ![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1654166748580.png)
			- ![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1654166716804.png)
			-
	-
	-
	- 16-完善登录功能_代码开发.
		- 跳吧，蒙圈
	-
	- 17-完善登录功能_功能测试.
		- 之后补吧，先把项目搞出来
	-
	- ### 18-新增员工_需求分析和数据模型.
		-
	-
	- 19-新增员工_梳理程序执行流程.
		- ![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1654170559776.png)
		-
	-
	- 20-新增员工_代码开发和功能测试.
		- 有点急躁，光看视频有什么用呢！
	- 21-新增员工_编写全局异常处理器.
		-
	- 22-新增员工_完善全局异常处理器并测试.
		- ![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1654172687562.png)
		- 巧了，我知识星球编号是1666.
		- 出现Bug，调试1小时。
	- 23-新增员工_小结.
		- https://www.bilibili.com/video/BV13a411q753?p=23
	-
	-
	- ### 24-员工信息分页查询_需求分析.
		- ![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1654175099033.png)
		-
	-
	- 25-员工信息分页查询_梳理程序执行过程.
		- 有点没状态了 https://www.bilibili.com/video/BV13a411q753?p=25&spm_id_from=pageDriver 值得借鉴
	-
	-
	- 26-员工信息分页查询_代码开发1.
		-
	- 27-员工信息分页查询_代码开发2.
	- 28-员工信息分页查询_功能测试.
	- 29-员工信息分页查询_补充说明.
	- 30-启用、禁用员工账号_需求分析.
	- 31-启用、禁用员工账号_分析页面按钮动态显示效果.
	- 32-启用、禁用员工账号_分析页面ajax请求发送过程.
	- 33-启用、禁用员工账号_代码开发和功能测试.
	- 34-启用、禁用员工账号_代码修复配置消息转换器.
	- 35-启用、禁用员工账号_再次测试.
	- 36-编辑工具信息_需求分析和梳理程序执行流程.
	- 37-编辑员工信息_页面效果分析和代码开发.
	- 38-编辑员工信息_功能测试.
	-
	-
	- 39-本章内容介绍.
	- 40-公共字段自动填充_问题分析.
	- 41-公共字段自动填充_代码实现并测试.
	- 42-公共字段自动填充_功能完善.
	- 43-新增分类_需求分析&数据模型&代码开发&功能测试.
	- 44-分类信息分页查询_需求分析&代码实现&功能测试.
	- 45-删除分类_需求分析&代码开发&功能测试.
	- 46-删除分类_功能完善.
	- 47-修改分类_需求分析&分析页面回显效果&代码开发&功能测试.
	- 48-本章内容介绍.
	- 49-文件上传下载介绍.
	- 50-文件上传下载_文件上传代码实现1.
	- 51-文件上传下载_文件上传代码实现2.
	- 52-文件上传下载_文件下载代码实现&测试.
	- 53-新增菜品_需求分析&数据模型.
	- 54-新增菜品_代码开发_准备工作&梳理交互过程.
	- 55-新增菜品_代码开发_查询分类数据.
	- 56-新增菜品_代码开发_接收页面提交的数据.
	- 57-新增菜品_代码开发_保存数据到菜品表和菜品口味表.
	- 58-新增菜品_代码开发_功能测试.
	- 59-菜品信息分页查询_需求分析.
	- 60-菜品信息分页查询_代码开发1.
	- 61-菜品信息分页查询_代码开发2.
	- 62-菜品信息分页查询_功能测试.
	- 63-修改菜品_需求分析&梳理交互过程.
	- 64-修改菜品_代码开发_根据id查询菜品信息和对应的口味信息.
	- 65-修改菜品_代码开发_测试数据回显.
	- 66-修改菜品_代码开发_修改菜品信息和口味信息.
	- 67-修改菜品_功能测试.
	- 68-本章内容介绍.
	- 69-新增套餐_需求分析&数据模型.
	- 70-新增套餐_代码开发_准备工作&梳理交互过程.
	- 71-新增套餐_代码开发_根据分类查询菜品.
	- 72-新增套餐_代码开发_服务端接收页面提交的数据.
	- 73-新增套餐_代码开发_保存数据到对应表.
	- 74-新增套餐_功能测试.
	- 75-套餐信息分页查询_需求分析&梳理交互过程.
	- 76-套餐信息分页查询_代码开发&功能测试.
	- 77-删除套餐_需求分析&梳理交互过程.
	- 78-删除套餐_代码开发&功能测试.
	- 79-本章内容介绍.
	- 80-短信发送_短信服务介绍和阿里云短信服务介绍.
	- 81-短信发送_阿里云短信服务.
	- 82-短信发送_代码开发_参照官方文档封装短信发送工具类.
	- 83-手机验证码登录_需求分析&数据模型.
	- 84-手机验证码登录_代码开发_梳理交互过程&修改LogInCheckFliter.
	- 85-手机验证码登录_代码开发_发送验证码短信.
	- 86-手机验证码登录_代码开发_登录校验.
	- 87-手机验证码登录_功能测试.
	- 88-本章内容介绍.
	- 89-导入用户地址簿相关功能代码_需求分析&数据模型&导入功能代码&功能测试.
	- 90-菜品展示_需求分析.
	- 91-菜品展示_代码开发_梳理交互过程.
	- 92-菜品展示_代码开发_修改DishController的list方法并测试.
	- 93-菜品展示_代码开发_创建SetmealController的list方法并测试.
	- 94-购物车_需求分析&数据模型&梳理交互过程&准备工作.
	- 95-购物车_代码开发_添加购物车.
	- 96-购物车_代码开发_查看购物车&清空购物车.
	- 97-用户下单_需求分析&数据模型.
	- 98-用户下单_代码开发_梳理交互过程&准备工作.
	- 99-用户下单_代码开发_1.
	- 100-用户下单_代码开发_2.
	- 101-用户下单_代码开发_3.
	- 102-用户下单_功能测试.
-