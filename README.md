# server-restart-project
 
  实际开发过程中，部署在服务器上的服务，没有集成k8s或者其他服务检测并重启的功能，当服务down掉时需要人为的去手动重启服务，为了能解决项目中存在的这种情况，自己实现了一个能减少自己工作的一个重启服务的小demo，目前还有很多需要根据实际情况来做更改来满足自己项目的实际需求。

##service-restart-server
  该项目是一个netty项目，目前主要提供了检测心跳以及服务重启功能。 序列化工具使用的是protobuf。
  该模块主要作用是将用户连接成功信息缓存到ServerManager类中，ServerManager类中提供了两个缓存容器，一个用来缓存channel，一个用来缓存需要重启的服务器信息，当检测到注册的连接发生断开时，自动触发重启断开服务机制。

##restart-client
  restart-client 项目主要是一个spring boot项目，其中该项目主要集成了netty，用于和service-restart-server项目保持通信。
  
  服务默认端口为：8099
  服务启动成功之后，需要发送一个请求到service-restart-server项目。
  请求方式：Get
  请求地址： http://ip:8099/service/register      直接发起请求，IP替换为部署服务器IP。
  以上操作完成时，当该服务down掉时，service-restart-server项目自动去重启down掉的服务。
  
  
该项目有很多可以扩展的地方，比如 集群部署service-restart-server项目，需要自己添加一些中间件如：RabbitMQ， 缓存持久化可以使用redis或者MySQL等。可以自己扩展protobuf中定义的message，来达到自己项目的要求。
  
  
