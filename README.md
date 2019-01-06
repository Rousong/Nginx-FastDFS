# Nginx-FastDFS
通过docker-compose一键搭建Nginx带动的FDFS服务 分布式文件上传系统

1.首先克隆项目到本地
>git clone https://github.com/Rousong/Nginx-FastDFS

2.修改docker-compose文件里面的TRACKER_SERVER ip地址

```
version: '2'
services:
  tracker-server:
    image: beaock/nginx-fastdfs
    #volumes:
      #- "./docker/fastdfs-tracker:/etc/fdfs/"
    network_mode: "host"
    command: "./tracker.sh" 
 
  storage-server:
    image: beaock/nginx-fastdfs
    volumes:
      - "./docker/storage_base_path:/data/fast_data"
      #- "./docker/store_path0:/fastdfs/store_path"
    environment:
      TRACKER_SERVER: "192.168.65.3:22122" <注意这里要改成自己tracker server的地址>
      GROUP_NAME: "M00"
    network_mode: "host"
    command: "./storage.sh"
  
  ```
    
    
    
### 查看TRACKER_SERVER容器ip地址的方法
1. 进入tracker容器内部
> docker exec -i -t <tracker容器的名字> /bin/bash
2. 一般要先执行
> apt-get install update
3. 安装ifconfig命令
> apt-get install net-tools
4. 然后输入ifconfig 然后看到eth0的ip地址就是tracker的ip地址了 端口默认是22122
```
eth0      Link encap:Ethernet  HWaddr 02:50:00:00:00:01
          inet addr:192.168.65.3  Bcast:192.168.65.255  Mask:255.255.255.0
          inet6 addr: fe80::50:ff:fe00:1/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:530127 errors:0 dropped:0 overruns:0 frame:0
          TX packets:231123 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:776445648 (776.4 MB)  TX bytes:13768969 (13.7 MB)
```

    
