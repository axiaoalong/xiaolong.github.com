# 总结
## 遇到的问题：
1，sql字段与类属性名称不一致问题：
解决方法一：在mapper.xml文件中配置
<resultMap type="com.xue.xi.pojo.User" id="user">
		<result property="adress" column="address" />
	</resultMap>
然后对应一下：


2  
一下第一天的学习：
创建了mybatis入门级程序，接着一个mapper动态代理的程序
入门级：
首先创建一个SqlMapConfig.xml文件，文件有头部，接下来是<configuration>标签，然后把jdbc事务什么的添加进去，然后创建一个User类，这个类实现Serializable接口，属性名称与数据库中表user属性名称及类型一致，然后创建一个user.xml映射文件，文件里面有一个头部，然后是<mapper>标签，下面是<select>等等的子标签，例如<select id="这个标签的id" parameterType="输入参数类型（也就是想查询的参数类型）" resultType="返回值类型，一般是一个User，或者是List<user>">
入门级程序写一个测试类，以后会用映射接口替代，他的原理是，首先创建一个字符串，值是"SqlMapConfig"，然后把这个字符串放到一个输入流里，然后创建一个SqlSessionFactory
1、mybatis的架构：

2、编写mybatis入门程序：
（1）、创建一个pojo也就是User 类，里面实现serializable接口,数据库字段属性，setget方法，重写toString方法
（2）、编写主配置文件：
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<properties resource="jdbc.properties"/>
	<!-- 别名 包以其子包下所有类   头字母大小都行-->
	<typeAliases>
<!-- 		<typeAlias type="com.itheima.mybatis.pojo.User" alias="User"/> -->
		<package name="com.itheima.mybatis.pojo"/>
	</typeAliases>
	<!-- 和spring整合后 environments配置将废除    -->
	<environments default="development">
		<environment id="development">
			<!-- 使用jdbc事务管理 -->
			<transactionManager type="JDBC" />
			<!-- 数据库连接池 -->
			<dataSource type="POOLED">
				<property name="driver" value="${jdbc.driver}" />
				<property name="url"
					value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8" />
				<property name="username" value="root" />
				<property name="password" value="root" />
			</dataSource>
		</environment>
	</environments>
	
	<!-- Mapper的位置  Mapper.xml 写Sql语句的文件的位置 -->
	<mappers>
<!-- 		<mapper resource="sqlmap/User.xml" class="" url=""/> -->
<!-- 		<mapper resource="sqlmap/User.xml" class="" url=""/> -->
<!-- 		<mapper class="com.itheima.mybatis.mapper.UserMapper" /> -->
<!-- 		<mapper url="" /> -->
		<package name="com.itheima.mybatis.mapper"/>
	</mappers>
</configuration>


即头文件，后面是<configuration></configuration>配置文件 然后<environments>标签是为了连接数据库，在与spring整合 之后用不到这个，spring中使用jdcp等连接数据库的接口。
（3）、写mapper映射文件，mapper映射文件中最主要的是sql语句sql语句写在<mapper>标签中如下：

最后把映射文件告诉主配置文件中，即
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mappers>
<mapper resource="主配置文件的子文件夹名称/User.xml" />
</mappers>
（4）、编写测试类：

首先用一个字符串接收xml文件，然后用一个字符流来读取这个文件
创建一个session工厂，吧User.xml参数 传进去
3、创建一个mabatis入门程序，用来实现sql模糊查询，与查id不同的是：

模糊查询用的是${}，其中${}与#{}区别是：后者拥有‘’单引号而前者没有，模糊查询大括号内必须有{value}，如果想用#查询，用法如下：
select * from user where username like "%"#{value}"%"
4、添加用户：
（1）测试类：需要注意事务要自己提交：sqlSession.commit();

（2）、User.xml：

5、更新用户：
测试类：

//更新用户
	@Test
	public void testUpdateUserById() throws Exception {
		//加载核心配置文件
		String resource = "sqlMapConfig.xml";
		InputStream in = Resources.getResourceAsStream(resource);
		//创建SqlSessionFactory
		SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(in);
		//创建SqlSession
		SqlSession sqlSession = sqlSessionFactory.openSession();
		
		//执行Sql语句 
		User user = new User();
		user.setId(29);
		user.setUsername("何炅292929");
		user.setBirthday(new Date());
		user.setAddress("222222sadfsafsafs");
		user.setSex("女");
		int i = sqlSession.update("test.updateUserById", user);
		sqlSession.commit();
	}
（2）、User.xml:
<!-- 更新 -->
	<update id="updateUserById" parameterType="com.itheima.mybatis.pojo.User">
		update user 
		set username = #{username},sex = #{sex},birthday = #{birthday},address = #{address}
		where id = #{id}
	</update>
6、删除用户
（1）、测试类：
//删除
	@Test
	public void testDelete() throws Exception {
		//加载核心配置文件
		String resource = "sqlMapConfig.xml";
		InputStream in = Resources.getResourceAsStream(resource);
		//创建SqlSessionFactory
		SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(in);
		//创建SqlSession
		SqlSession sqlSession = sqlSessionFactory.openSession();
		
		sqlSession.delete("test.deleteUserById", 29);
		sqlSession.commit();
	}
}
（2）、User.xml:
<!-- 删除 -->
	<delete id="deleteUserById" parameterType="Integer">
		delete from user 
		where id = #{vvvvv}
	</delete>
7、原始dao开发：思想：之前的测试类是应该dao层的东西，现在将他们放到dao里面，其中有一些代码是重复的，
（1）、创建一个dao及其实现类将他们放到一个dao的包内
写一个dao接口，写一个实现类，daoimp
8、动态代理：
（1）、还是前面用的User，User.xml，SqlMapper.xml
不同的是用dao层变成一个接口，成为一个新的dao：接口规范如下：

（2）、创建一个测试类，充当services层：

-----------------------------------------------------------------------------------------------------------------
9、SqlMapper.xml文件的属性：

总结：
（1）prorerties，指向一个配置文件，属性有 resource
<properties resource="jdbc.properties"/>
（2）settings，二级缓存，现在不使了
（3）typeAliases,别名，可以指定某一个类名什么的，这个配置是整个项目都会其反映的：
<typeAliases>
<!-- 		<typeAlias type="com.itheima.mybatis.pojo.User" alias="User"/> -->
		<package name="com.itheima.mybatis.pojo"/>
</typeAliases>
有两个属性，typeAlias type 指别名类型，package指的是包名，用来自动检索包下的所有类型
（4）mappers  :
映射文件，除了resource之外，还有两个属性，分别是：
1.class  用来指向映射接口的，要求此刻的映射接口和映射文件是同名字的并且放在同一个包下。
2、url  指向映射文件的绝对路径的，一般不会用的
3.package，指向包名，此刻将检索此包下的所有映射接口和映射文件，前提是所有映射接口和映射文件同一个名字，并且放在同一个包下。企业一般使用这种指定
例子：

<!-- Mapper的位置  Mapper.xml 写Sql语句的文件的位置 -->
	<mappers>
<!-- 		<mapper resource="sqlmap/User.xml" class="" url=""/> -->
<!-- 		<mapper resource="sqlmap/User.xml" class="" url=""/> -->
<!-- 		<mapper class="com.itheima.mybatis.mapper.UserMapper" /> -->
<!-- 		<mapper url="" /> -->
		<package name="com.itheima.mybatis.mapper"/>
	</mappers>


-------------------------------------------------------------------------------------------------------------------
高阶篇：


一、用包装类，例如QueryVo包装类，用它来实现一个username的模糊查询，

foreach标签，用来解决输入多个值的，下面的代码解释就是
select * from user where  id in( 这里面是输入的多个参数)



if标签，
include标签是前面的<sql>标签用来存放部分sql字段的。







关联表用法，左关联

一对一映射：<association>标签，一对一映射

一对多映射：
<association>标签，一对一映射，ofType 是集合list每个元素的类型



00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
SPRING_MVC

一、入门级程序
1、入门程序之前端控制器的配置：


2、在web.xml中配置springmvc配置文件：
首先写上容器及监听器：

然后在applicationConfig.xml中配置添加jdbc事务

然后配默认文件-springmvc.xml：与spring使用同一个容器：contextConfigLocation

3、接下来创建一个控制器类，打上注解，其中@requestMapping中的值是访问的一个触发，也就是action。然后new一个视图，设置他的路径，也就是把jsp路径给他，然后返回视图。


然后在springmvc.xml中将这个打了注解的控制器扫描出来：

二、springMVC三大组件：


 
三、这个注解驱动相当于上面两个bean

2、视图解释器

把这个视图url的前后缀放到springmvc.xml中，如下：

3、参数绑定:

简单的基本类型绑定：

乱码问题：

案例：

高阶篇：


一、 数组类型的绑定，可以用Integer[] ids数组，也可以用QueryVo vo 来绑定，在jsp页面的写法如下：

用List<Items> itemsList 集合来放进QueryVo vo里面，在jsp里面这样做

二、requestMapping注解标签：
1）requestMapping(value="/asdf.action")value值是路径名称，value属性可以省略
2）requestMapping(value="/asdf.do" ,method="RequestMethod.POST")方法限定为post方法 
3）@RequestMapping(value="/adf.do" ,method={RequestMethod.POST，RequestMethod.GET})多个方法限定
4）提取重复url路径的前半部分eg: RequestMapping(value="/asdf/asdfsdf.do"),这时可以将/asdf作为头部提取到类前面：

5）多请求设置：
@RequestMapping(value={/asdf/asdfsadf,/asdfasd/asdfsadfasdf})
访问这两个路径都可以到这个请求
三、Controller层的视图和数据写法三种：
1）ModelAndView  无敌的，视图和数据都能返回  
public ModelAndView adEite(){
ModelAndView mv=new ModelAndView();
mv.addObject("itemList", items)//前者与jsp的name一致，后者传的items   的list集合
mv.setViewName("/asdf/asdf.action");//映射视图路径
return mv；
}
2）使用String类型返回值，这是官方推荐，符合视图数据分离 

3）void 返回。适用于不需要返回视图的情况


重定向：到列表视图   内部转发就是将redirect 改成forward

四：
异常处理：自定义异常，并把它抛到控制台中：

其中，自定义一个异常处理器，然后实现HandlerExceptionResolver接口，最后交由springmvc来管理：
<bean  class="com.xue.xi.exception.ExceptingSelf"> </bean >

自定义异常：在可能出现异常的地方自定义一个异常，把他用类实现一下，然后在异常控制器中把异常判断一下：

四、上传图片：
其中MultipartFile中的 名字与jsp中name的名字必须一致

把其中的图片接口在springmvc中配置一个实现类

在jsp中应该加入：

五、配置拦截器：
1）在springmvc.xml文件中将拦截器配置好，定位到实现类中，

2）实现类中重写方法，分别是方法执行前，方法执行后和视图渲染后的拦截器，如果配置多个拦截器，则
111第一个拦截器 方法前拦截开启不放行后，其余别的都不执行了，
222第一个放行后第二个方法执行前不放行，则只有第一个  方法前 执行，第二个方法前执行，然后第一个视图渲染后执行
333  只要有前面的方法前执行完成后，必执行视图渲染后的方法  
333只要有一个方法执行前拦截不放行后，所有方法执行后都不执行
三）做一个用户登录验证，其中思路：
拦截器里面方法前执行操作：将现在执行的uri拿出来判断，看看里面是否有/Login.action，如果有，则放行，如果没有，判断username是否是空，如果是空值，不放行，跳转登录界面，如果不是空值，放行。

package com.xue.xi.intercept;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

public class MyInterceptor implements HandlerInterceptor {

	//方法执行前拦截
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response,
			Object obj) throws Exception {
		String requestUri=request.getRequestURI();//拿到当前url地址
		if(!requestUri.contains("/Login")){//判断当前url地址中是否包含/Login
//在session中拿到username
			String userna=(String) request.getSession().getAttribute("namename");

			if(null==userna){
				response.sendRedirect(request.getContextPath()+"/Login.action");
				return false;
			}
			return true;
		}
		return true;
	}
	//方法执行后拦截
	public void postHandle(HttpServletRequest arg0, HttpServletResponse arg1,
			Object arg2, ModelAndView arg3) throws Exception {
		// TODO Auto-generated method stub	
	}
	//视图渲染后拦截
	public void afterCompletion(HttpServletRequest arg0,
			HttpServletResponse arg1, Object arg2, Exception arg3)
			throws Exception {
		// TODO Auto-generated method stub
	}
}

然后在控制类中给session注值
//简单登录界面：
	@RequestMapping(value = "/Login.action",method=RequestMethod.POST)
	public String toLogin(String username,
			HttpSession httpSession
			) throws Exception{
		httpSession.setAttribute("namename", username);//给session填值
		
		return "redirect:/xue/show.action";//redirect重定向
		
	}
	@RequestMapping(value = "/Login.action",method=RequestMethod.GET)
	public String toLogi() throws Exception{
		
		return "Login";
		
	}


ssm  xml 总结：
首先，sqlMapConfig.xml:
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

<typeAliases>
        <!-- 2. 指定扫描包，会把包内所有的类都设置别名，别名的名称就是类名，大小写不敏感 -->
        <package name="com.xue.xi.pojo" />
    </typeAliases>
    <!-- 扫描包 -->
    <mappers>
        <package name="com.xue.xi.mapper" />
    </mappers>
</configuration>
然后ItemsMapper.xml:
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.xue.xi.mapper.UserMapper">
    
    <select id="findIdTest" parameterType="Integer" resultType="com.xue.xi.pojo.Items">
        select * from items where id= #{id}
    </select>
</mapper>
然后applicationContext.xml:
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
    http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
    http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
    http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd">


	<context:property-placeholder location="classpath:db.properties" />

	<!-- 数据库连接池 -->
	<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"
		destroy-method="close">
		<property name="driverClassName" value="${jdbc.driver}" />
		<property name="url" value="${jdbc.url}" />
		<property name="username" value="${jdbc.username}" />
		<property name="password" value="${jdbc.password}" />
		<property name="maxActive" value="10" />
		<property name="maxIdle" value="5" />
	</bean>

	<!-- Mybatis的工厂 -->
	<bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource" />
		<!-- 核心配置文件的位置 -->
		<property name="configLocation" value="classpath:SqlMapConfig.xml" />
	</bean>

	<!-- Mapper动态代理开发 扫描 -->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<!-- 基本包 -->
		<property name="basePackage" value="com.xue.xi" />
	</bean>

	
	<!-- 创建实例 <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer"> 
		<property name="basePackage" value="com.xue.xi.mapper.UserMapper"></property> 
		</bean> -->
</beans>
然后springmvc.xml:
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">

	<!-- 扫描Controller -->
	<context:component-scan base-package="com.xue.xi"></context:component-scan>
	<!-- 处理器映射器 -->
	<!-- <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/> -->
	<!-- 处理器适配器 -->
	<!-- <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/> -->
	<!-- 注解驱动 -->
	
	<mvc:annotation-driven conversion-service="conversionServiceFactoryBean" />

	<!-- 配置Conveter转换器 转换工厂 （日期、去掉前后空格）。。 -->
	<bean id="conversionServiceFactoryBean"
		class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
		<!-- 配置 多个转换器 -->
		<property name="converters">
			<list>
				<bean class="com.xue.xi.conversion.DateConveter" />
			</list>
		</property>
	</bean>
	
	<!-- 配置自定义异常 -->
	<bean class="com.xue.xi.exception.SelfException" />
	<!-- 配置拦截器 -->
	<mvc:interceptors>
	   <mvc:interceptor >
	       <mvc:mapping path="/**"/>
	       <bean class="com.xue.xi.intercept.MyInterceptor"/>
	   </mvc:interceptor>
	</mvc:interceptors>
	<!-- 视图解释器 -->
	<bean
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/jsp/" />
		<property name="suffix" value=".jsp" />
	</bean>

</beans>
然后web.xml:
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
	<display-name>ssm_selftest</display-name>
	<welcome-file-list>
		<welcome-file>index.jsp</welcome-file>
		<welcome-file>index.html</welcome-file>
		<welcome-file>index.htm</welcome-file>
		<welcome-file>default.jsp</welcome-file>
		<welcome-file>default.html</welcome-file>
		<welcome-file>default.htm</welcome-file>
	</welcome-file-list>
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext.xml</param-value>
	</context-param>
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	<!-- 处理POST提交乱码问题 -->
  <filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  
  <filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>*.action</url-pattern>
  </filter-mapping>
  
	<!-- 前端控制器 -->
	<servlet>
		<servlet-name>springmvc</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:springmvc.xml</param-value>
		</init-param>
	</servlet>
	<servlet-mapping>
		<servlet-name>springmvc</servlet-name>
		<url-pattern>*.action</url-pattern>
	</servlet-mapping>
</web-app>














