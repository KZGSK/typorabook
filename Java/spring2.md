# spring2

## 工厂

使用工厂来实现解耦。

~~~java
//创建工厂的方式
//方式1，使用xml
ApplicationContext ctx = new ClassPathXmlApplicationContext("/acpplicationContext.xml");
类 对象 = (类)ctx.getBean("id");

//xml配置
<bean id="person" class="com.xxx.Person,scope="singleton|prototype,lazy="false"/>

//方式2，使用注解    
//方式2.1，使用@Componet注解+XML
//id默认为person，
@Componet
@Scope(singleton|prototype)
@Lazy(false|true)
public Class Person{
    
}
//xml配置:告知Spring扫描指定包及其子包对应的注解
<context:component-scan base-package="com.xxx"/>
xml扫描可替换为
@ComponentScan(basePackages = "com.xxx")
//方式2.2，使用@Bean、@Configuration注解
//id为方法名，类为返回类型
ApplicationContext ctx = new AnnotionConfigApplicationContext("com.xxxx")
@Configuration
@ComponentScan(basePackages = "com.xxx")
public class AppConfig{
    @Bean
    public Person person(){
        return new Person();
    }
}
~~~

### 复杂对象

~~~java
//方式1实现FactoryBean接口
public class ConnectionFactoryBean implements FactoryBean<Connection> {
    @Override
    public Connection getObject() throws Exception {
        Class.forName("com.mysql.jdbc.Driver");
        Connection coon = DriverManager.getConnection("jdbc:mysql://localhost:3306/mysql","root","123"); 
        return coon;
    }

    @Override
    public Class<?> getObjectType() {
        return Connection.class;
    }
    @Override
    public boolean isSingleton() {
        return false;
    }
}
//方式2实例工厂
public class ConnectionFactory {
    public Connection getConnection(){
        //xxxx
        return conn;
    }
}
//方式3静态工厂
public class StaticConnectionFactory {
    public static Connection getConnection(){
        //xxxx
        return conn;
    }
}
~~~

## 注入

### JDK注入

~~~java
//方式1使用xml
public Class Person{
    private String name;
    private Integer age;
    set、get方法
    public Person(String name, int age){
        this.name = name;
        this.age = age;
    }
}
<bean id="person" class="com.xxx.Person>
    <property name="name">
    	<value>张三</value>
    </property>
    <property name="age">
    	<value>18</value>
    </property>
</bean>
//构造注入
<bean id="person" class="com.xxx.Person>
   <constructor-arg>
        <value>李四</value>
    </constructor-arg>
    <constructor-arg>
        <value>34</value>
    </constructor-arg>
</bean>

//方式2.1 @Value注解注入+xml
//创建properties 文件指定参数值(name = 张三 age=18)
//XML添加文件
<context:property-placeholder location="/init.properties"/>
属性上加 @Value("${key}") 注解、

//方式2.2 @Bean注解注入
@Configuration
@PropertySource("classpath:/init.properties")
public class AppConfig{
    @Value("${name}")
    private String name;
    @Value("${age}")
    private Integer age;
    @Bean
    public Person person(){
        Person person = new Person();
        person.setName(name)
        person.setage(age)   
        return person;
    }
}

~~~

### 自定义类型注入

~~~java
public class UserServiceImpl implement UserService{
    private UserDAO userDAO;
    set、get方法
}
//方式1 xml
<bean id="userDao" class="com.xxx.UserDaoImpl"/>
<bean id="userServiceImpl" class="com.xxx.UserServiceImpl">
    <property name="userDAO">
    	<ref bean="userDao"/>
    </property>
</bean>
//方式2.1 @Autowired
//将@Autowired放置在成员变量上或者set方法上
//方式2.2 @Autowired() @Qualifier("userDAOImpl")
//方式2.3 @Resouce("userDAOImpl")
//方式2.4 @Bean
@Bean
public UserDAO userDAO() {
  return new UserDAOImpl();
}
@Bean
public UserService userService(UserDAO userDAO) {
  UserServiceImpl userService = new UserServiceImpl();
  userService.setUserDAO(userDAO);
  return userService;
}
~~~

## AOP

添加额外功能。并将额外功能与核心功能分离

~~~java
//创建原始类 - 定义额外功能(实现接口)代理类 - 定义切入点 - 组装
//原始类
public class UserServiceImpl implements UserService{
    @Override
    public void register(User user) {
        System.out.println("UserServiceImpl.register");
    }

    @Override
    public boolean login(String name, String password) {
        System.out.println("UserServiceImpl.login");
        return true;
    }
}
//代理类
public class Around implements MethodInterceptor {
    @Override
    public Object invoke(MethodInvocation invocation) throws Throwable {
        System.out.println("------ 额外功能 log -----");
        Object ret = invocation.proceed();
        return ret;
    }
}
//切入点
<aop:config>
   <aop:pointcut id="pc" expression="execution(* *(..))"/>
</aop:config>
//组装
<aop:advisor advice-ref="Around" pointcut-ref="pc"/>
~~~

###注解版的AOP编程

```java
//1. 原始对象
@Component
public class UserServiceImpl implements UserService{

}
//2. 创建切面类 （额外功能 切入点 组装切面）
@Aspect
@Component
public class MyAspect {
    @Around("execution(* login(..))")
    public Object arround(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("----aspect log ------");
        Object ret = joinPoint.proceed();
        return ret;
    }
}
//3. Spring的配置文件中
<aop:aspectj-autoproxy />   //替换为：
@EnableAspectjAutoProxy --->// 加在配置Bean上 
```

在同一个业务类中，业务方法间相互调用时，只有最外层的方法,加入了额外功能(内部的方法，通过普通的方式调用，运行的都是原始方法)。如果想让内层的方法也调用代理对象的方法，就要实现AppicationContextAware获得工厂，进而获得代理对象。

## 持久层整合

### Mybatis

主体是一个核心配置文件和mapper的开发

1核心配置文件mybatis-config.xml

2创建一个MyBatisUtils工具类，（获SqlSession对象）

3创建一个映射接口如UserMapper，创建一个映射文件UserMapper.xml。

4在总配置文件中配置映射文件。

5编写测试插入数据代码（通过UserMapper接口调用UserDao中的方法）。

**mybatis-config.xml**

配置MyBatis的运行环境，包含MyBatis的事务管理器transactionManager=JDBC; 数据源(配置连接池)dataSource=druid，并设置连接数据库操作：驱动、url、账号、密码。

配置映射文件，将UserMapper.xml加载，并指定配置类<mapper class="org.cjw.mapper.UserMapper" />

```java
//读取配置文件，创建工厂对象
InputStream in = Resources.getResourceAsStream("mybatis-config.xml")
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(in);
//获取SqlSession对象
SqlSession session = sqlSessionFactory.openSession();
//获取Mapper接口代理对象
UserMapper userMapper = session.getMapper(UserMapper.class);
//执行方法
User user = userMapper.selectUserById(1);
```

```xml
<mapper namespace="org.cjw.mapper.UserMapper">
    
 <select id="selectUserById" resultType="User">
 		select * from tb_user where id = #{id}
 </select>
    
 </mapper>
```

### 整合

配置文件（ApplicationContext.xml) 的相关配置

```xml
<!--连接池-->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
  <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
  <property name="url" value="jdbc:mysql://localhost:3306/suns?useSSL=false"></property>
  <property name="username" value="root"></property>
  <property name="password" value="123456"></property>
</bean>

<!--注册SqlSessionFactoryBean 为了创建SqlSessionFactory-->
<bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
  <property name="dataSource" ref="dataSource"></property>
  <property name="typeAliasesPackage" value="com.baizhiedu.entity"></property>
  <property name="mapperLocations">
    <list>
      <value>classpath:com.baizhiedu.mapper/*Mapper.xml</value>
    </list>
  </property>
</bean>

<!--注册MapperScannerConfigure 用于创建Dao的实现类-->

<bean id="scanner" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
  <property name="sqlSessionFactoryBeanName" value="sqlSessionFactoryBean"></property>
  <property name="basePackage" value="com.baizhiedu.dao"></property>
</bean>
```