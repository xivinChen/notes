### docker 安装单机版elasticsearch 命令
# 拉去镜像

``docker pull docker.elastic.co/elasticsearch/elasticsearch:6.0.1``

# 启动

``docker run -p 9201:9201 -p 9301:9301 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:6.0.1``

## mac下查看elasticearch的安装信息

``brew info elastisearch``
