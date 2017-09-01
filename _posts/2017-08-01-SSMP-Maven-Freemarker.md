---
layout: post
title:  SSM框架——详细整合教程（Spring+SpringMVC+MyBatisPlus+Maven）
categories: java
tags: ssm
description: SSM框架——详细整合教程（Spring+SpringMVC+MyBatisPlus+Maven）
---

* content
{:toc}

## 说明
  记录SSM 框架整合、使用Maven构建，说M实际上是MybatisPlus、是一种对Mybatis封装好的框架、在不影响Mybatis原生的情况下，进行的一种扩展。

  > 框架支持Freemarker 和 JSP 双View展示（优先找Freemarker）。  

## 项目环境  
  JDK : 1.7  
  SpringMVC : 4.2.5  
  Spring : 4.2.5
  Mybatis Plus : 2.0.5
  IntelliJ IDEA : 2016.2.5  
  LomBok : 1.16.8  
  Veolcity : 1.7 `只用来mybatis反向生成代码`  
  freemarker : 2.3.23  
  
 LomBok 是一种能自动生成Get & Set 等方法、大大减少开发时代码量、保持代码简洁。  
 查看LomBok 操作：[https://projectlombok.org/](https://projectlombok.org/)

<!--more-->

`Github 项目地址：`[https://github.com/Jandaes/liujilu-ssm](https://github.com/Jandaes/liujilu-ssm)
>   - Mybatis Plus 自动会引入Mybatis 的jar
>   - 使用Maven Tomcat Plugin部署运行 ([Intellij IDEA 中如何部署项目到 Tomcat？](http://liujilu.com/2017/03/27/Idea-deploy-Tomcat/))
>   - 可通过Eclipse 或MyEclipse部署运行、不仅限于IDEA


## 配置文件  
### pom.xml :
```xml
    <build>
        <finalName>liujilu</finalName>
        <plugins>
            <!--添加Tomcat7 maven 插件-->
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.2</version>
                <configuration>
                    <uriEncoding>UTF-8</uriEncoding>
                </configuration>
            </plugin>
        </plugins>
    </build>
	<!--配置全局版本属性-->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <spring.version>4.2.5.RELEASE</spring.version>
        <junit.version>4.12</junit.version>
        <druid.version>1.0.18</druid.version>
        <fastjson.version>1.2.8</fastjson.version>
        <mybaitsplus.version>2.0.5</mybaitsplus.version>
        <mysql.version>5.1.38</mysql.version>
        <log4j.version>1.2.17</log4j.version>
        <slf4j.version>1.7.19</slf4j.version>
        <aspectjweaver.version>1.8.8</aspectjweaver.version>
        <shiro.version>1.3.1</shiro.version>
    </properties>
    
    <!-- Spring MVC -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>${spring.version}</version>
        <type>jar</type>
        <scope>compile</scope>
    </dependency>
    
	<!--参考项目源码-->
	<!--......-->
```

### web.xml
```xml
    <!-- SpringMVC 过滤 -->
    <servlet>
        <servlet-name>spring-mvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring/spring-mvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>spring-mvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    
    <!-- 加载Spring配置 -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring/spring.xml,classpath:spring/spring-*.xml</param-value>
    </context-param>
    <!-- 启动监听器 -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!--监听器出用于主要为了解决java.beans.Introspector导致内存泄漏的问题-->
    <listener>
        <listener-class>org.springframework.web.util.IntrospectorCleanupListener</listener-class>
    </listener>
```
从项目根目录`/`开始拦截所有的请求、全部交给Springmvc管理，包括静态资源（`css,js,image`），如果项目有引入静态资源也被拦截的话、需要在`springmvc.xml`加入：
`
<mvc:resources location="/image/" mapping="/image/**"/> 
<mvc:resources location="/css/" mapping="/css/**"/> 
<mvc:resources location="/js/" mapping="/js/**"/>
`
> classpath:spring/spring-*.xml  : 加载所有spring目录下spring- 开头的xml文件

### Spring.xml 
```xml
    <!-- 自动扫描 -->
    <context:component-scan base-package="com.liujilu.service.*"/>
    <!-- 开启注解 -->
    <context:annotation-config/>

    <!-- 引入配置文件 -->
    <bean id="propertyConfigurer"  class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
        <property name="location" value="classpath:jdbc.properties" />
    </bean>
```

### SpringMVC.xml
```xml
    <!-- 自动扫描该包，使SpringMVC认为包下用了@controller注解的类是控制器,不扫描Service，使用Spring中的Service -->
    <context:component-scan base-package="com.liujilu.controller">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Service"/>
    </context:component-scan>
    <!-- 处理器映射器 -->
    <mvc:annotation-driven />
    <!-- 引入静态资源 -->
    <mvc:default-servlet-handler/>
    <!-- 视图解析器（ViewResolver）配置 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- 前缀 -->
        <property name="prefix" value="/WEB-INF/pages/"/>
        <!-- 后缀 -->
        <property name="suffix" value=".jsp"/>
    </bean>

    <!-- 视图解析器（ViewResolver）配置 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- 前缀 -->
        <property name="prefix" value="/WEB-INF/pages/"/>
        <!-- 后缀 -->
        <property name="suffix" value=".jsp"/>
    </bean>
    <!-- 添加Freemarker解析器 -->
    <!-- Freemarker配置 -->
    <bean id="freemarkerConfig"
          class="org.springframework.web.servlet.view.freemarker.FreeMarkerConfigurer">
        <property name="templateLoaderPath" value="/WEB-INF/views/" />
        <property name="defaultEncoding" value="utf-8" />
        <property name="freemarkerSettings">
            <props>
                <prop key="template_update_delay">10</prop>
                <prop key="locale">zh_CN</prop>
                <prop key="datetime_format">yyyy-MM-dd</prop>
                <prop key="date_format">yyyy-MM-dd</prop>
                <prop key="number_format">#.##</prop>
            </props>
        </property>
    </bean>
    <!-- 视图解析器（ViewResolver）配置 -->
    <!--视图解释器 -->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.freemarker.FreeMarkerViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.freemarker.FreeMarkerView"></property>
        <property name="suffix" value=".ftl" />
        <property name="contentType" value="text/html;charset=utf-8" />
        <property name="exposeRequestAttributes" value="true" />
        <property name="exposeSessionAttributes" value="true" />
        <property name="exposeSpringMacroHelpers" value="true" />
        <property name="requestContextAttribute" value="request" />
        <property name="order" value="1" />
    </bean>
```
`/WEB-INF/pages/` : SpringMVC 的返回页面、只返回`.jsp`结尾的文件
`/WEB-INF/views/`:Freemarker 的模版页面、只返回`.ftl`结尾的文件
> Freemarker文件后缀可自己编写、比如：`*.html`


### spring-mybatis.xml
```xml
	<!--数据源配置查看项目源码-->
    <!-- Spring整合Mybatis，更多查看文档：http://mp.baomidou.com -->
    <bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!-- 自动扫描Mapping.xml文件 -->
        <property name="mapperLocations" value="classpath:mybatis/xml/*.xml"/>
        
        <!-- 配置 Mybatis 配置文件（可无） 
        <property name="configLocation" value="classpath:mybatis/mybatis-config.xml"/>
         -->
         <!-- 配置包别名 -->
        <property name="typeAliasesPackage" value="com.liujilu.model"/>
        <property name="plugins">
            <array>
                <!-- 分页插件配置 -->
                <bean id="paginationInterceptor" class="com.baomidou.mybatisplus.plugins.PaginationInterceptor">
                    <property name="dialectType" value="mysql"/>
                </bean>
            </array>
        </property>
	    <!-- 全局配置注入 -->
	    <property name="globalConfig" ref="globalConfig" />
	</bean>
	
	<!-- 定义 MP 全局策略 -->
	<bean id="globalConfig" class="com.baomidou.mybatisplus.entity.GlobalConfiguration">
	    <!-- 
			AUTO->`0`("数据库ID自增")
		 	INPUT->`1`(用户输入ID")
			ID_WORKER->`2`("全局唯一ID")
			UUID->`3`("全局唯一ID")
		-->
	    <property name="idType" value="2" />
		<!--
			MYSQL->`mysql`
			ORACLE->`oracle`
			DB2->`db2`
			H2->`h2`
			HSQL->`hsql`
			SQLITE->`sqlite`
			POSTGRE->`postgresql`
			SQLSERVER2005->`sqlserver2005`
			SQLSERVER->`sqlserver`
		-->
		<property name="dbType" value="mysql"/>
		
		<!-- Oracle需要添加该项 -->
	    <!-- <property name="dbType" value="oracle" /> -->
	    
	    <!-- 全局表为下划线命名设置 true -->
	    <property name="dbColumnUnderline" value="true" />
	</bean>
```
整合Mybatis plus 配置

### SysUser.java
```java
/**
 * <p>
 * 		用户信息表
 * </p>
 *
 * @since 2017-07-31
 */
@TableName("sys_user")
@Data
@AllArgsConstructor
@NoArgsConstructor
public class SysUser extends Model<SysUser> {

    private static final long serialVersionUID = 1L;

    /**
     * 编号
     */
	@TableId(value="id", type= IdType.ID_WORKER)
	private String id;
    /**
     * 姓名
     */
	private String name;
    /**
     * 昵称
     */
	private String nickname;
    /**
     * 邮箱
     */
	private String email;
    /**
     * Q号码
     */
	private String number;
    /**
     * 密码
     */
	private String password;
    /**
     * 创建时间
     */
	@TableField("create_time")
	private Date createTime;
    /**
     * 最后登录时间
     */
	@TableField("last_login_time")
	private Date lastLoginTime;
    /**
     * 状态：0 锁定、 1 正常
     */
	private Integer status;
	@Override
	protected Serializable pkVal() {
		return this.id;
	}

}
```
`@Data` :  会自动生成Set & Get方法
`@AllArgsConstructor` : 会自动生成构造方法
`@NoArgsConstructor` : 会自动生成无参构造方法
`@TableId(type= IdType.ID_WORKER)`:主键生成策略、查看`spring-mybatis.xml`

> PS : 如果有构造方法的话、必须要写无参构造方法


### SysUserController.java
```java
@Controller
@RequestMapping("/")
public class SysUserController {
    private Logger logger = Logger.getLogger(SysUserController.class);

    @Autowired
    private SysUserService userService;

    /**
     * 返回到JSP
     * @return
     */
    @RequestMapping("index")
	public ModelAndView index(){
        ModelAndView mv = new ModelAndView("index");
        logger.info("进来了");
        List<SysUser> userList = userService.selectList(new EntityWrapper<SysUser>());
        for (SysUser user:userList) {
            System.out.println(user.toString());
        }
        mv.addObject("userList",userList);
        return mv;
    }

    /**
     * 返回到FreeMarker
     * @param model
     * @return
     */
    @RequestMapping("/freemarker/index")
    public String index(Model model){
        logger.info("进来了Freemarker 处理器");
        List<SysUser> userList = userService.selectList(new EntityWrapper<SysUser>());
        for (SysUser user:userList) {
            System.out.println(user.toString());
        }
        model.addAttribute("userList",userList);
        return "indexs";
    }
}
```

### MpGenerator.java
```java
/**
 * <p>
 * 	代码生成器请勿轻易操作
 * 	操作时请取消 pom.xml 中的 velocity 注释
 * </p>
 */
public class MpGenerator {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		AutoGenerator mpg = new AutoGenerator();
		// 全局配置
        GlobalConfig gc = new GlobalConfig();
        gc.setOutputDir("D://cache");//生成存放地址
        gc.setFileOverride(true);
        gc.setActiveRecord(true);
        gc.setEnableCache(false);// XML 二级缓存
        gc.setBaseResultMap(true);// XML ResultMap
        gc.setBaseColumnList(false);// XML columList
        gc.setAuthor("D.Yang");
        
        // 自定义文件命名，注意 %s 会自动填充表实体属性！
        gc.setMapperName("%sMapper");
        gc.setXmlName("%sMapper");
        gc.setServiceName("%sService");
        gc.setServiceImplName("%sServiceImpl");
        gc.setControllerName("%sController");
        mpg.setGlobalConfig(gc);
        
        // 数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setDbType(DbType.MYSQL);
        dsc.setTypeConvert(new MySqlTypeConvert(){
            // 自定义数据库表字段类型转换【可选】
            @Override
            public DbColumnType processTypeConvert(String fieldType) {
                System.out.println("转换类型：" + fieldType);
                return super.processTypeConvert(fieldType);
            }
        });
        
        
        dsc.setDriverName("com.mysql.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("");
        dsc.setUrl("jdbc:mysql://127.0.0.1:3306/liujilu?characterEncoding=utf8");
        mpg.setDataSource(dsc);
        
        // 策略配置
        StrategyConfig strategy = new StrategyConfig();
        strategy.setTablePrefix(new String[] {""});// 此处可以修改为您的表前缀
        strategy.setNaming(NamingStrategy.underline_to_camel);// 表名生成策略
        //strategy.setInclude(new String[]{"sys_order"});//生成指定表
        mpg.setStrategy(strategy);
        
        // 包配置
        PackageConfig pc = new PackageConfig();
        pc.setParent("com");
        pc.setModuleName("liujilu");
        pc.setEntity("model");
        mpg.setPackageInfo(pc);
        // 执行生成
        mpg.execute();
	}

}
```
该工具类、自动通过连接数据库生成对应的java文件、包括：
`Controller、model、Service、ServiceImpl、Mappler、Mapperxml`
只要修改配置里面的数据库连接地址、以及修改生成代码的存放地址就可以使用。

## 注
具体更多的MybatisPlus方法操作、查看：[http://git.oschina.net/baomidou/mybatis-plus](http://git.oschina.net/baomidou/mybatis-plus)

> 以上代码为主要代码、更多代码查看Github 源码


参考网站：[http://liujilu.com/](http://liujilu.com/)

