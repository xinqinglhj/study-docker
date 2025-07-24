## docker compose语法
将docker run命令中的参数，以yaml文件的形式，进行配置，
实际和docker run命令一样，启动容器

[Compose file reference | Docker Docs](https://docs.docker.com/reference/compose-file/)



### docker compose -f compose.yaml up -d

- 通过compose文件启动docker容器
- 修改文件后，可以重复执行命令，更新容器

### docker compose -f compose.yaml down -v --rmi all
- 如果不加 -v -rmi 参数，默认不会删除卷
- --rmi all 删除镜像 (-rmi 后需要指定服务名或all 所有服务)
- -v 删除数据卷

![image-20250722220744824](.\images\image-20250722220744824.png)

![image-20250722220832764](.\images\image-20250722220832764.png)