## redis主从集群
- 创建两个容器redis01 redis02，一主一从，实现读写分离
- 创建docker自定义网络mynet
- 使用目录挂载，将redis持久化的数据备份到主机
- 修改redis01 redis02的配置文件（环境变量）
![](./images/docker-16-01.png)
![](./images/docker-16-02.png)

### bitnami版本的redis镜像
- 支持主从集群
- 支持以环境变量的形式配置redis 
- 地址：[bitnami/redis - Docker Image | Docker Hub](https://hub.docker.com/r/bitnami/redis)
### 自定义网络
docker network create mynet 
### 主节点
docker run -d -p 6379:6379 \
-v /app/rd1:/bitnami/redis/data \
-e REDIS_REPLICATION_MODE=master \
-e REDIS_PASSWORD=123456 \
--network mynet --name redis01 \
bitnami/redis

-v 挂载目录
-e 环境变量
--network 网络
--name 容器名

![image-20250721213710407](.\images\image-20250721213710407.png)

### 从节点
docker run -d -p 6380:6379 \
-v /app/rd2:/bitnami/redis/data \
-e REDIS_REPLICATION_MODE=slave \
-e REDIS_MASTER_HOST=redis01 \
-e REDIS_MASTER_PORT_NUMBER=6379 \
-e REDIS_MASTER_PASSWORD=123456 \
-e REDIS_PASSWORD=123456 \
--network mynet --name redis02 \
bitnami/redis



![image-20250721214558371](.\images\image-20250721214558371.png)

![image-20250721214913821](.\images\image-20250721214913821.png)

### 启动失败
docker ps 查看无正在运行的redis01容器  
docker logs redis01 查看日志
发现是挂载目录没有权限
解决：

chmod -r 777 /app/rd1

777:表示所有人可读可写可执行

-r : 递归操作目录及子目录

执行命令，修改目录权限之后，重启redis01

docker restart redis01

![](./images/docker-16-03.png)



