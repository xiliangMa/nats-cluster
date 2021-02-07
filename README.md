# nats-streaming-cluster
- Nats-Streaming Cluster By Docker Compose
- Nats-Streaming Cluster By Kubernetes

#### Support Docker-Compose Deploy
nats + nats-streaming 集群方案
> 1 通过nats + nats-streaming 搭建3节点nats集群，nats提供服务; \
> 2 支持认证;\
> 3 nats-streaming 提供节点和消息持久化;
- 启动
```bash
git clone https://github.com/xiliangMa/nats-cluster.git
cd docker-compose
docker network create nats-net
docker-compose -f nats&nats-streaming.yaml up -d
```

```.env
> docker-compose -f nats&nats-streaming.yaml ps
  Name                       Command               State                            Ports                          
--------------------------------------------------------------------------------------------------------------------------
root_nats-streaming1_1   /nats-streaming-server --s ...   Up      4222/tcp, 8222/tcp                                      
root_nats-streaming2_1   /nats-streaming-server --s ...   Up      4222/tcp, 8222/tcp                                      
root_nats-streaming3_1   /nats-streaming-server --s ...   Up      4222/tcp, 8222/tcp                                      
root_nats1_1             /nats-server -m 8222 --use ...   Up      0.0.0.0:4222->4222/tcp, 6222/tcp, 0.0.0.0:8222->8222/tcp
root_nats2_1             /nats-server -m 8222 --use ...   Up      0.0.0.0:4223->4222/tcp, 6222/tcp, 0.0.0.0:8223->8222/tcp
root_nats3_1             /nats-server -m 8222 --use ...   Up      0.0.0.0:4224->4222/tcp, 6222/tcp, 0.0.0.0:8224->8222/tcp
```
- 测试

使用 [nats-bench](https://docs.nats.io/nats-tools/natsbench#installing-and-running-the-benchmark-utility)
```.bash
 nats-bench -s nats://nats:P@ssw0rd@localhost:4222,nats://nats:P@ssw0rd@localhost:4223,nats://nats:P@ssw0rd@localhost:4224 np 1 -ns 1 -n 1 -ms 10 test
```

nats-streaming 集群方案
> 通过nats-streaming 搭建3节点集群 \
> 支持认证（暂没有配置可以参考方案1） \
> 消息持久化

- 启动
```.bash
git clone https://github.com/xiliangMa/nats-cluster.git
cd docker-compose
docker network create nats-net
docker-compose -f nats-streaming.yaml up -d
```

```.bash
> docker-compose -f nats-streaming.yaml ps
        Name                       Command               State                Ports              
-------------------------------------------------------------------------------------------------
nats-cluster_nats-1_1   /nats-streaming-server -sc ...   Up      0.0.0.0:4222->4222/tcp, 8222/tcp
nats-cluster_nats-2_1   /nats-streaming-server -sc ...   Up      0.0.0.0:4223->4222/tcp, 8222/tcp
nats-cluster_nats-3_1   /nats-streaming-server -sc ...   Up      0.0.0.0:4224->4222/tcp, 8222/tcp
```

- 测试

使用 [nats-bench](https://docs.nats.io/nats-tools/natsbench#installing-and-running-the-benchmark-utility)
```.bash
 nats-bench -s nats://localhost:4222,nats://localhost:4223,nats://localhost:4224 -np 1 -ns 1 -n 1 -ms 10 test
```