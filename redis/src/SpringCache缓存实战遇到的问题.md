#  我原以为，springboot加缓存那是分分钟的事情，不就几个注解嘛！
## 接到任务信心十足，下一秒就打脸了，说说遇到的问题吧！

### 总结的说我遇到的困难基本都离不开 缓存同步！！！

> 也许你会说，缓存同步不就是在更新和删除时把缓存清空就完美解决了吗？
>年轻的我也是这么想，缓存同步解决方法的确是这样，但是没想到的是很多情况下，
>一条数据的修改会引起很多关联的数据发生变化，但是缓存不清的话就无法同步，也就是你一个表的
>更新可能要清除很多相关模块的缓存！
>

### 举个例子，三个表，用户 产品 订单
> 分别缓存单个用户，用户订单，订单产品
>
> 那么，当用户信息更新时，由于订单关联了用户，因此不仅要更新用户缓存还要更新订单缓存，
>
> 然后再到产品信息更新了，同理订单也要跟着清空缓存，
>
> 而实际业务中可能远不止这么简单的管理，那么这样算起来整个系统的缓存工作量是相当大的！！！
>

## 下面是我引发的问题

> 业务描述：给一个用户提供者加缓存，其中主要接口有：通过id查询用户信息，通过用户名查询用户信息，通过电话号码查询用户信息
>，通过id修改用户密码，通过id修改电话号码，通过id修改用户等，要求给他们加入缓存！
>
>问题分析：首先是三个查询加缓存，认真思考过会发现同一个用户可能缓存了三个缓存（id,username,tel)，而他们的键不一样都是值都是一样的，
>于是引发了第一个问题，可不可以存在一个值对应多个键。
>
>其次：在缓存同步上，不难发现，再通过用户id更新用户密码时并不能清空通过用户名和电话号码查询用户信息的缓存
>（因为传进来的只有id和password，所以不知道要情况哪些缓存）！
>
>解决方案：一个值对应多个键是不能实现的！
>在缓存同步问题上有两种解决办法：一是每次更新把通过用户名和电话号码的查询缓存全部清空；
>二是通过用户名和电话号码查询的接口不用加缓存！
>
>
>sudo yum install -y java-1.8.0-openjdk-devel.x86_64
>
scp -i /Users/bosen/Downloads/redis-test-ami.pem /Users/bosen/.m2/repository/bosenCCR/bs-ccr-aws-redis/1.0.0/bs-ccr-aws-redis-1.0.0.jar ec2-user@ec2-13-56-44-214.us-west-1.compute.amazonaws.com:/home/ec2-user/bs-ccr  