#MyBatis

MyBatis是支持定制化sql，存储过程以及高级映射的持久层框架。

主体是一个核心配置文件和mapper的开发

1核心配置文件mybatis-config.xml

2创建一个MyBatisUtils工具类，（获SqlSession对象）

3创建一个映射接口如UserMapper，创建一个映射文件UserMapper.xml。

4在总配置文件中配置映射文件。

5编写测试插入数据代码（通过UserMapper接口调用UserDao中的方法）。

##mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!-- dtd约束 -->
<!DOCTYPE configuration
        PUBLIC "-//MyBatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!-- 根元素: 用于配置MyBatis -->
<configuration>
    <!-- 配置MyBatis的运行环境 ,可以配置多个环境,但是一次只能使用一个
        default属性 : 当前使用的环境 ,使用下面环境的id 即可  -->
    <environments default="dev_mysql">
        <!-- 环境配置  id 属性,唯一标识当前环境 -->
        <environment id="dev_mysql">
            <!-- 配置MyBatis事务管理器
                type属性 :  事物类型
                    JDBC  使用事务(正常提交commit,异常回滚事务 rollback) 默认
                    MANAGED : 不使用事务
              -->
            <transactionManager type="JDBC"/>
            <!-- 配置MyBatis的数据源
                type : 配置连接池
                    POOLED :MyBatis内置的一个连接池(默认)
                    后期都交给spring管理了,配置 dbcp连接池,阿里巴巴的 druid连接池
             -->
            <dataSource type="POOLED">
                <!-- 连接数据库的操作 -->
                <!-- 数据库驱动 -->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <!-- 连接数据库的url -->
                <property name="url" value="jdbc:mysql://localhost:3306/users"/>
                <!-- 连接数据库账号 -->
                <property name="username" value="root"/>
                <!-- 连接数据库密码 -->
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <!-- 配置映射文件 -->
    <mappers>
        <!-- <mapper resource="org/cjw/mapper/UserMapper.xml"/>--> <!--指定一个配置文件-->
        <!--<package name="org.cjw.mapper"/>--> <!-- 指定一个包，包含多个配置文件或者配置类，推荐 -->
        <mapper class="org.cjw.mapper.UserMapper" /> <!-- 指定一个配置类 -->
    </mappers>
</configuration>
```

##MyBatisUtil工具类

MyBatisUtil工具类的作用主要用于读取配置文件，创建工厂对象，提供创建SqlSession数据库操作对象的方法。

```java
public class MyBatisUtil {

    private MyBatisUtil() {}

    private static SqlSessionFactory sqlSessionFactory;

    static {
        try(InputStream in = Resources.getResourceAsStream("mybatis-config.xml")) {
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(in);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static SqlSession openSession() {
        return sqlSessionFactory.openSession();
    }
}
```

##创建映射接口

### 创建数据库表对应的实体类

```java
public class User {

    private Long id;
    private String name;
    private Integer age;
    private String email;
    private String password;
// set、get方法
```

### 创建一个操作接口

```java
public interface UserMapper {

 /*
        使用MyBatis的动态代理开发编写代码遵循四个原则
        1.映射文件的namespace命名空间的值必须是对应接口的全限定名。
        2.映射文件的对应功能 id值必须等于映射接口中方法的名称。
        3.映射文件的参数类型必须和接口中方法的参数类型一致。
        4.映射文件查询的返回结果类型必须和接口的方法的返回 数据类型一致，
        DML操作返回的受影响的行数，除外。
     */
 void insert(User user);
}
```

### 创建表对应的映射文件 :UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//MyBatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- 配置映射
namespace : 命名空间(通俗说法: 给当前映射文件的唯一标识:起一个唯一的名字)
 -->
<mapper namespace="org.cjw.mapper.UserMapper">
    <!-- 新增操作
        id: 当前功能的唯一标识,和接口方法同名
        parameterType ： 参数的类型
        useGeneratedKeys：是否返回数据库生成的主键 true是/false否
        keyProperty : 数据库主键对应java的pojo对象的属性
        keyColumn : 数据表的主键列名
     -->
    <insert id="insert" parameterType="org.cjw.pojo.User" keyProperty="id" keyColumn="id">
        insert into tb_user (name, age, email, password) values (#{name}, #{age}, #{email}, #{password});
    </insert>
</mapper>
```

```java
@Test
public void testSelectUserById() {
    //获取SqlSession对象
    SqlSession session = MyBatisUtil.openSession(); 
    //获取Mapper接口代理对象
    UserMapper userMapper = session.getMapper(UserMapper.class);
    //执行方法
    User user = userMapper.selectUserById(1);
    System.out.println(user.getName());
}
```

## **ResultMap** **手动映射**

myBatis的查询结果集都是自动映射封装的，单行查询将数据库一条数据封装成对应的Java对象。多行查询，先将每一行封装成对象，再将每个对象添加到集合中，最后返回一个List集合对象。

必须保证查询结果集和pojo对象的属性名相同，否则无法自动封装。查询结果集名称和pojo对象属性不同的映射封装需使用手动映射封装 **<resultMap>**标签

```xml
<select id="selectUserById" resultMap="user_map"> 
    select id u_id, name u_name, age u_age, email u_email, password u_password from tb_user where id = #{id}
</select>

<resultMap id="user_map" type="User">
    <id property="id" column="u_id" />  主键字段映射
    <result property="name" column="u_name"/> 普通字段映射
    <result property="age" column="u_age"/>
    <result property="email" column="u_email"/>
    <result property="password" column="u_password"/>
</resultMap>
```

## **别名typeAliases标签**

在UserMapper.xml文件中User无论是作为参数还是作为查询返回数据类型，都需要写上全限定名，实际可以写上简单类名即可，但是需要配置别名。

MyBatis框架提供了两种别名机制，一种是自定义别名，一种是内置别名。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
 PUBLIC "-//MyBatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- 配置映射
namespace : 命名空间(通俗说法: 给当前映射文件的唯一标识:起一个唯一的名字)
 -->
<mapper namespace="org.cjw.mapper.UserMapper">
 <!-- 新增操作
        id: 当前功能的唯一标识,和接口方法同名
        parameterType ： 参数的类型
        useGeneratedKeys：是否返回数据库生成的主键 true是/false否
        keyProperty : 数据库主键对应java的pojo对象的属性
        keyColumn : 数据表的主键列名
     -->
 <insert id="insert" parameterType="User" keyProperty="id" keyColumn="id">
 insert into tb_user (name, age, email, password) values (#{name}, #{age}, #{email}, #{password});
 </insert>
    <select id="selectUserById" resultType="User">
 		select * from tb_user where id = #{id}
 </select>
 <!-- 多行查询
       resultType : 无论是多行查询还是单行查询,返回的结果类型都是对应的JavaBean的类型
    -->
 <select id="selectAll" resultType="User">
 select * from tb_user
 </select>

    <delete id="deleteUserById">
 delete from tb_user where id = #{id}
 </delete>

    <update id="updateUserById" parameterType="User">
 update tb_user set
        name = #{name}, age = #{age}, email = #{email}, password = #{password}
        where id = #{id}
 </update>

</mapper>
 
```