### docker 安装单机版elasticsearch 命令
# 拉去镜像

``docker pull docker.elastic.co/elasticsearch/elasticsearch:6.0.1``

# 启动

``docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:6.0.1``