# Install
* docker.io，docker-ce的区别
* 检查: `sudo systemctl status docker`
* 故障:docker.socket故障引起docker进程有问题
* 安装参考google搜索官方文档

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

# Images
NOTE: 如果空间不够，修改默认的/var/lib/docker的路径
```
docker pull xx
docker images

# 清理空间
docker image rm xxx
# 其他清楚残余image的命令要十分小心!!!!!!
```
# Runtime

```
docker ps
docker commit <container-id>  xxx:xxx
```
# Run
* 不同版本，将GPU从host导入container的参数是不一样的
  * docker.io: `--gpus all`
  * docker-ce: `xxx`
 
* Host的ethx导入container的参数`--network host`
* `--rm`退出后不保存，不然还要清理

```
sudo docker run --gpus all --network host -it --rm --shm-size=5g -v /home/ops:/xxx nvidia/cuda:12.4.1-devel-ubuntu20.04-v2 /bin/bash
```
