# docker快速启动solr服务

```angular2
//查询solr镜像
docker search solr

//拉取最新的solr服务
docker pull solr

//启动solr镜像服务
docker run --name my_solr -d -p 8983:8983 -t solr

//最后一步，检查是否安装成功
访问浏览器：127.0.0.1:8983 即可进入控制台
```