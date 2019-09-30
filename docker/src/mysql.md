# docker 安装mysql
```docker run -p 3306:3306 --name mysql --env MYSQL_ROOT_PASSWORD=123zxc -d mysql:5.7```

# docker 安装redis
```aidl
docker run -d --name myredis -p 6379:6379 redis --requirepass "mypassword"
```
