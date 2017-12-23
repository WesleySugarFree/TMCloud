# TMCloud
    TMCloud是Trailer Music Cloud的简称。
 
>  *Trailer music (a subset of production music) is the background music used for film previews, which is not always from the film's soundtrack. The purpose of this music is to complement, support and integrate the sales messaging of the mini-movie that is a film trailer. Because the score for a movie is usually composed after the film is finished (which is much after trailers are released), a trailer will incorporate music from other sources. Sometimes music from other successful films or hit songs is used as a subconscious tie-in method.*
>  ————From [Wikipedia][1]

## 待定内容...
    目前国内关于版权音乐、预告片音乐甚者说科幻史诗预告片配乐的动态较少，我也知道有一些团队正在制作属于中国的科幻史诗预告片配乐，但是我还是想凭借自己的力量能够稍微推动预告片音乐在国内的发展。不为别的原因，只因喜欢。<br/>
    如果你对该开源项目感兴趣，你可以扫描下面的二维码和我一起交流 ^_^ <br/>
    <img src="http://oosk9q3p6.bkt.clouddn.com/wechatTJ.png" width = "300px"/>

## 技术选型
| 模块 | 说明 | 技术点 | 
| - | :-: | :-: | 
| tmcloud-discovery-eureka-server(port:8761/8762) | 服务注册与发现。 | Spring Cloud Eureka | 
| tmcloud-admin-ui(port:10000) | 服务监控界面。 | Spring Boot Admin | 
| tmcloud-api-gataway(port:10001) | api网关服务提供者。 | Spring Cloud Zuul | 
| tmcloud-auth(port:11111) | auth认证中心。 | Spring Cloud Security、JWT | 
| tmcloud-hystrix-dashboard-with-turbine(port:10002) | 服务容错监控面板。 | Spring Cloud Hysrtix、Turbine | 
| tmcloud-provider-user(port:9901) | 用户服务提供者。 | Spring Cloud Eureka、Mybatis | 
| tmcloud-provider-song(port:9902) | 歌曲服务提供者。 | Spring Cloud Eureka、Mybatis | 
| tmcloud-provider-singer(port:9903) | 歌手服务提供者。 | Spring Cloud Eureka、Mybatis | 
| tmcloud-provider-album(port:9904) | 专辑服务提供者。 | Spring Cloud Eureka、Mybatis | 
| tmcloud-provider-usercomment(port:9905) | 用户评论服务提供者。 | Spring Cloud Eureka、Mybatis | 
| tmcloud-provider-type(port:9906) | 歌曲类型服务提供者。 | Spring Cloud Eureka、Mybatis | 
| tmcloud-provider-aggregate-musicalbum(port:9911) | 歌曲专辑聚合服务提供者。 | Spring Cloud Eureka、Spring Data JPA | 
| tmcloud-bus-rabbitmq | 事件、消息总线服务。 | Spring Cloud Bus | 

## 运行
 ***前提条件：***
 >将以下内容添加到或根据实际情况修改hosts文件：
 ```
 127.0.0.1 discovery gateway localhost
 ```
 - discovery：为Eureka注册中心的hostname
 - gateway：为网关的hostname
 
 ```
  mvn clean
  mvn clean package -Dmaven.test.skip=true # 将每个项目打包
 ```
 - 1. 初始化数据库 
 
 ```
    docker-compose up -d #后台通过docker-compose服务编排程序启动(./docker-compose.yml)mysql数据库镜像（该镜像导入了相关数据）
 ```
    如果出现如下信息则代表运行成功：
    > MySQL Community Server 5.7.20 is running.
      mysql_1  | 2.开始导入数据....
      mysql_1  | 3.导入数据完毕....
      mysql_1  | MySQL Community Server 5.7.20 is running.
      mysql_1  | 4.开始修改密码....
      mysql_1  | host user
      mysql_1  | localhost    mysql.session
      mysql_1  | localhost    mysql.sys
      mysql_1  | localhost    root
      mysql_1  | 5.修改密码完毕....
      mysql_1  | MySQL Community Server 5.7.20 is running.
      mysql_1  | mysql容器启动完毕,且数据导入成功
      
***数据库连接用户名和密码可在./privileges.sql中查看***

- 2. 依次将每个module里面的Dockerfile构建成镜像
    例如:
    ```
      cd ./tmcloud-discovery-eureka-server/
      docker build -t tmcloud-discovery-docker . #将当前目录下的Dockerfile构建成镜像名为'tmcloud-discovery-docker'的一个镜像
      docker run -p 8761:8761 -d tmcloud-discovery-docker #运行上面构建的镜像,同时将容器的8761端口映射到宿主机的8761端口
      打开浏览器访问 http://localhost:8761或者http://127.0.0.1:8761/ 可以查看运行效果
    ```
    

  [1]: https://en.wikipedia.org/wiki/Trailer_music
