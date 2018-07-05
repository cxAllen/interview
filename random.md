王节 321283199502251220	15021879939

cd C:\DevSoftware\kafka_2.11-0.10.1.0\bin\windows
zookeeper-server-start.bat ..\..\config\zookeeper.properties &


SpringMVC异步处理请求原理：
1.一个请求到达dispatchServlet,执行请求派发时，判断controller的返回值类型是否为callable或deferedResult.如果是的话就执行相应的handler执行器。
根据returnType的选择handler。
2.在异步handler执行逻辑里会有线程池来处理该异步请求，还有执行完成、超时处理的逻辑。
3.当callable或deferedResult处理完成，就再调用servlet来讲请求响应视图返回。

应用SOA化

也就是业务的服务化。比如原来单机支撑了整个电商网站，现在对整个网站进行拆解，分离出了订单中心、用户中心、库存中心。对于订单中心，有专门的数据库存储订单信息，用户中心也有专门的数据库存储用户信息，库存中心也会有专门的数据库存储库存信息。这时候如果要同时对订单和库存进行操作，那么就会涉及到订单数据库和库存数据库，为了保证数据一致性，就需要用到分布式事务。
http://t10.baidu.com/it/u=363569670,2933589489&fm=170&s=338DF70612BA4C291E528A640300307A&w=640&h=334&img.JPEG

MyBatis初始化流程

首先会创建SqlSessionFactory建造者对象，然后由它进行创建SqlSessionFactory。这里用到的是建造者模式，建造者模式最简单的理解就是不手动new对象，而是由其他类来进行对象的创建。由建造者开始读取配置文件，在configuration节点下会依次解析properties/settings/.../mappers等节点配置。在解析environments节点时，会根据transactionManager的配置来创建事务管理器，根据dataSource的配置来创建DataSource对象，这里面包含了数据库登录的相关信息。在解析mappers节点时，会读取该节点下所有的mapper文件，然后进行解析，并将解析后的结果存到configuration对象中。然后根据configuration创建sqlsessionfactory对象；

MyBatis的SQL查询流程

SQL语句的执行才是MyBatis的重要职责，该过程就是通过封装JDBC进行操作，然后使用Java反射技术完成JavaBean对象到数据库参数之间的相互转换，这种映射关系就是有TypeHandler对象来完成的，在获取数据表对应的元数据时，会保存该表所有列的数据库类型.
sqlseesion执行selectList-->executor.query(parameters)--->handler进行数据封装转换resultSet
执行器executor在query操作中，优先会查询缓存是否命中，命中则直接返回，否则从数据库中查询,并记录结果至缓存。一级缓存

![image](https://github.com/cxAllen/interview/blob/master/mybatis.png)<br/>

mybatis缓存

Mybatis的一级缓存是指SqlSession。一级缓存的作用域是一个SqlSession。Mybatis默认开启一级缓存。
在同一个SqlSession中，执行相同的查询SQL，第一次会去查询数据库，并写到缓存中；第二次直接从缓存中取。当执行SQL时两次查询中间发生了增删改操作，则SqlSession的缓存清空。

Mybatis的二级缓存是指mapper映射文件。二级缓存的作用域是同一个namespace下的mapper映射文件内容，多个SqlSession共享。Mybatis需要手动设置启动二级缓存。
在同一个namespace下的mapper文件中，执行相同的查询SQL，第一次会去查询数据库，并写到缓存中；第二次直接从缓存中取。当执行SQL时两次查询中间发生了增删改操作，则二级缓存清空。
