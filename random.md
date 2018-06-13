
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
