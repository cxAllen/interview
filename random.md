
cd C:\DevSoftware\kafka_2.11-0.10.1.0\bin\windows
zookeeper-server-start.bat ..\..\config\zookeeper.properties &


SpringMVC异步处理请求原理：
1.一个请求到达dispatchServlet,执行请求派发时，判断controller的返回值类型是否为callable或deferedResult.如果是的话就执行相应的handler执行器。
根据returnType的选择handler。
2.在异步handler执行逻辑里会有线程池来处理该异步请求，还有执行完成、超时处理的逻辑。
3.当callable或deferedResult处理完成，就再调用servlet来讲请求响应视图返回。
