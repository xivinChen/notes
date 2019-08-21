# 用EC2连接Redis 命令
``sudo yum install gcc``
```$xslt
wget http://download.redis.io/redis-stable.tar.gz
tar xvzf redis-stable.tar.gz
cd redis-stable
make distclean  // Ubuntu systems only
make
```

```src/redis-cli -c -h mycachecluster.eaogs8.0001.usw2.cache.amazonaws.com -p 6379```

# aws安装 jdk
```sudo yum install -y java-1.8.0-openjdk-devel.x86_64```

# 往aws上传 文件
```
// /Users/bosen/Downloads/redis-test-ami.pem秘药文件的位置
// .../1.0.0/bs-ccr-aws-redis-1.0.0.jar 要上传的文件
// 后面接的是主机地址和文件路径
scp -i /Users/bosen/Downloads/redis-test-ami.pem /Users/bosen/.m2/repository/bosenCCR/bs-ccr-aws-redis/1.0.0/bs-ccr-aws-redis-1.0.0.jar ec2-user@ec2-13-56-44-214.us-west-1.compute.amazonaws.com:/home/ec2-user/bs-ccr ```
scp -i /Users/bosen/Downloads/redis-test-ami.pem /Users/bosen/.m2/repository/bosenCCR/bs-ccr-aws-redis/1.0.0/bs-ccr-aws-redis-1.0.0.jar ec2-user@ec2-13-56-44-214.us-west-1.compute.amazonaws.com:/home/ec2-user/bs-ccr 