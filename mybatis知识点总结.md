## 基础：

1、 概念：Java当中的一个持久层框架。
2、 特点、优势：
（1）把java代码和SQL代码做了一个完全分离。
（2）良好支持复杂对象的映射（输入映射、输出映射）
（3）使用动态SQL，可以预防SQL注入。
3、 原理：
（1）创建mybatis-config.xml配置文件
（2）创建sqlSessionFactory
（3）编写数据库表对应的实体类
（4）创建mybatis的sql映射文件，在这个文件中，把实体类的属性和数据库表的列联系起来，并且可以编写sql语句
（5）从sqlSessionFactory的实例获取session
（6）session内部通过Executor执行器执行操作。Executor会用到mapped statement对象，这个对象是对数据库存储的封装，包括：sql语句、输入参数、输出结果类型
（7）关闭session

## MyBatis缓存：

1、概念：
（1）一级缓存：一级缓存是SqlSession（会话）级别的缓存。在操作数据库时需要构造sqlSession对象，在对象中有一个数据结构（HashMap）用于存储缓存数据。不同的sqlSession之间的缓存数据区域（HashMap）是互相不影响的。
（2）二级缓存：二级缓存是mapper级别的缓存，多个SqlSession去操作同一个Mapper的sql语句，多个SqlSession可以共用二级缓存，二级缓存是跨SqlSession的。（二级缓存的原理和一级缓存原理一样，第一次查询，会将数据放入缓存中，然后第二次查询则会直接去缓存中取。但是一级缓存是基于 sqlSession 的，而 二级缓存是基于 mapper文件的namespace的，也就是说多个sqlSession可以共享一个mapper中的二级缓存区域，并且如果两个mapper的namespace相同，即使是两个mapper，那么这两个mapper中执行sql查询到的数据也将存在相同的二级缓存区域中）
它们的关系如图：!



![clipboard.png](https://segmentfault.com/img/bVbdNeZ?w=775&h=312)

2、一级缓存的使用：
我们在一个 sqlSession 中，对User表根据id进行两次查询，查看他们发出sql语句的情况：
（1）第一次发起查询用户id为1的用户信息，先去找缓存中是否有id为1的用户信息，如果没有，从数据库查询用户信息。得到用户信息，将用户信息存储到一级缓存中。
（2）如果中间sqlSession去执行commit操作（执行插入、更新、删除），则会清空SqlSession中的一级缓存，这样做的目的为了让缓存中存储的是最新的信息，避免脏读。
（3）第二次发起查询用户id为1的用户信息，先去找缓存中是否有id为1的用户信息，缓存中有，直接从缓存中获取用户信息。

![clipboard.png](https://segmentfault.com/img/bVbdNfv?w=558&h=332)

3、二级缓存的使用：

![clipboard.png](https://segmentfault.com/img/bVbdNfC?w=637&h=375)

4、注意事项：
（1）如果设置了cacheEnabled=true; 那么MyBatis在执行查询的时候，先查看二级缓存（全局缓存）是否有查询的结果，如果有，直接返回缓存的结果；如果没有，再执行真正的查询，把查询的结果放到缓存中，再把结果返回给用户。
（2）二级缓存：mybatis中，每一个mapper都可以有一个二级缓存。使用<cache>节点配置。
（3）二级缓存使用场景：

```
对于访问多的查询请求且用户对查询结果实时性要求不高，此时可采用mybatis二级缓存技术降低数据库访问量，提高访问速度，业务场景比如：耗时较高的统计分析sql、电话账单查询sql等。实现方法如下：通过设置刷新间隔时间，由mybatis每隔一段时间自动清空缓存，根据数据变化频率设置缓存刷新间隔flushInterval，比如设置为30分钟、60分钟、24小时等，根据需求而定。
 mybatis二级缓存对细粒度的数据级别的缓存实现不好，比如如下需求：对商品信息进行缓存，由于商品信息查询访问量大，但是要求用户每次都能查询最新的商品信息，此时如果使用mybatis的二级缓存就无法实现当一个商品变化时只刷新该商品的缓存信息而不刷新其它商品的信息，因为mybaits的二级缓存区域以mapper为单位划分的，当一个商品信息变化会将所有商品信息的缓存数据全部清空。解决此类问题可能需要在业务层根据需求对数据有针对性缓存。
```

## 语法：

1、#{}和${}
（1）#{}是一个占位符，接收输入参数，参数类型可以是基本类型、POJO、HashMap
（2）${}是一个拼接符号，可能导致sql注入，不建议使用。
2、resultMap标签：
（1）结果集映射的标签
（2）resultMap：

```
A、type：指定结果集中保存的实体类的类型
B、id：resultMap标签的标识
C、autoMapping：值范围true|false，设置是否启动自动映射功能。自动映射功能就是自动查找与字段名小写同名的属性名，并调用setter方法。设置为false之后，需要在resultMap内明确注明映射关系才会调用对应的setter方法。
```

（3）id：用于设置主键字段与领域模型属性的映射关系
（4）result：用于设置普通字段与领域模型属性的映射关系
3、sql标签：用来封装sql语句或者sql片段

MyBatis的DAO开发方法：
1、 使用mapper代理方法：
（1） 编写mapper.java 的接口类
（2） 编写mapper.xml
2、 编写接口类，需要遵守一些规范，MyBatis可以自动生成接口类的DAO实现类。规范有：
（1） mapper.xml中namespace需要等于mapper接口类的地址
（2） mapper接口类的方法的名称等于mapper.xml中statement（对应sql语句）的id
（3） mapper接口类的输入参数类型等于mapper.xml中statement的parameterType指定的类型
（4） mapper接口类的返回参数类型等于mapper.xml中statement的resultType指定的类型
3、

MyBatis和Hibernate的本质区别：
1、 Hibernate不需要写sql，通过hql或者Hibernate直接生成sql。Mybatis需要写sql。
2、 Mybatis适合于需求经常改动的项目，因为它的sql由程序员生成，容易改动。Hibernate适合于需求改动较少的项目。

MyBatis和Ibatis的本质区别：
1、MyBatis简化了编码过程，不需要写DAO的实现类。直接写一个DAO的接口和一个配置文件。MyBatis自动生成DAO实现类。

嵌套查询 及 嵌套结果查询：
嵌套查询：使用2次查询，然后在MyBatis（内存）中进行拼装。可能引起N+1问题。
嵌套结果查询：使用1次查询，然后将结果进行拼装。

#### Mybatis 中resultMap,resultType区别

基本映射 ：（resultType）使用resultType进行输出映射，只有查询出来的列名和pojo中的属性名一致，该列才可以映射成功。（数据库，实体，查询字段,,这些全部都得一一对应）

高级映射 ：（resultMap） 如果查询出来的列名和pojo的属性名不一致，通过定义一个resultMap对列名和pojo属性名之间作一个映射关系。（高级映射，字段名称可以不一致，通过映射来实现）

参考：https://www.cnblogs.com/Darkqueen/p/10682039.html

#### MyBatis中如何表示Decimal

```
使用java.math.BigDecimal
```

#### MyBatis中多个参数时，如何自定义变量名

使用@param 基于注解来指定 ，把可以把封闭到map中，用#{keyname}来表示

```
String getXXXByUserIdAndExamId(@Param("userId") Integer userId, @Param("examId") Integer examId);
```

#### MyBatis中如何插入多行

参数传入list,里面有foreach进

```xml
int addRItem(@Param(value = "list") List<GreenBeanMsg> list);
    
<insert id="addRItem" parameterType="java.util.List">
    insert into lzf_rental_item_detailsl
    (   
        id,
        rentalInfoId,
        itemName,
        number,
        remark
    )
    values
    <foreach collection="list" item="item" index= "index" separator =",">
    (
        #{item.id},
        #{item.rentalInfoId},
        #{item.itemName},
        #{item.number},
        #{item.remark}
    )
    </foreach>

</insert>
```

#### Mybatis中多个参数时，有String又有List时如何传

将String和List放入map中，再对map中的list进行遍历

```
List<String> list = new ArrayList<String>();
Map<String, Object> map = new HashMap<String, Object>();

list.add("1");
list.add("2");
map.put("list", list); 
map.put("str", "0");
```

#### Mybatis中如何继承原有的类

使用extends 来继承原有的，通常是在原有的实体中增加自定义的列。这样就不用再重新写原来的属性了。

```xml
  <resultMap extends="BaseResultMap" id="ResultMapWithBLOBs" type="com.caiyun.entity.TblGpExam">
    <result column="rule" jdbcType="LONGVARCHAR" property="rule" />
</resultMap>
```

#### Mybatis报错nested exception

xxx.xml]'; nested exception is org.apache.ibatis.builder.BuilderException: Error parsing Mapper XML. The XML location is 'file

检查xxx.xml中，看是不是有指定的类型转化错误 ，一定是在这个文件中哪里设置的不对，仔细检查排除。

#### MyBatis报No enum constant org.apache.ibatis.type.JdbcType.String

```
查看xml中字段的类型，通过是对应的类型设置错误 。
```

