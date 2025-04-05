# Distributed Training Notes
## Check network before training
* hardware topology, `nvida-smi topo -m`
* hardware to software interface map `ibdev2netdev`, example as below

```
$ ibdev2netdev
mlx5_0 port 1 ==> eth0 (Up)
mlx5_1 port 1 ==> eth1 (Up)
mlx5_2 port 1 ==> eth2 (Up)
mlx5_3 port 1 ==> eth3 (Up)
mlx5_4 port 1 ==> eth4 (Up)
mlx5_5 port 1 ==> eth5 (Up)
mlx5_6 port 1 ==> eth6 (Up)
mlx5_7 port 1 ==> eth7 (Up)
mlx5_8 port 1 ==> eth8 (Up)
mlx5_9 port 1 ==> eth9 (Up)
```
* check ethx's IP address(need IP address(RoCE))

## Nccl test to check network links and bandwidth
* mpi use ssh protocol need to set no password access firstly. Ref: https://blog.csdn.net/MrKingloveyou/article/details/136074767
```
# 将id_rsa.pubcopy从被访问节点，copy到访问节点
# In one host
ssh-keygen -t rsa
ssh-copy-id -i ~/.ssh/id_rsa.pub xx.xx.xx.xx
# In another host
ssh-keygen -t rsa
ssh-copy-id -i ~/.ssh/id_rsa.pub xx.xx.xx.xx

$ cat ~/.ssh/
authorized_keys  config           id_rsa           id_rsa.pub 
```

* environment variables for NIC
```
export NCCL_IBEXT_DISABLE="1"
export NCCL_IB_DISABLE="0"
export NCCL_IB_GID_INDEX="3"
export NCCL_IB_HCA="mlx5_1:1,mlx5_2:1,mlx5_3:1,mlx5_4:1,mlx5_5:1,mlx5_6:1,mlx5_7:1,mlx5_8:1"
export NCCL_IB_PCI_RELAXED_ORDERING="1"
export NCCL_IB_RETRY_CNT="7"
export NCCL_IB_TIMEOUT="23"
export NCCL_SOCKET_IFNAME="eth0"  # MUST
```

* run nccl-tests(if some environments variables are set above, some variables below could be removed)
  * Ignore nccl-test compilation process here.
```
mpirun -np 16 \
        -H xx.xx.xx.xx:8,xx.xx.xx.xx:8 \
        --allow-run-as-root -bind-to none -map-by slot \
        -x NCCL_DEBUG=INFO \
        -x NCCL_IB_GID_INDEX=3 \
        -x NCCL_IB_DISABLE=0 \
        -x NCCL_SOCKET_IFNAME=eth0 \
        -x NCCL_NET_GDR_LEVEL=2 \
        -x NCCL_IB_QPS_PER_CONNECTION=4 \
        -x NCCL_IB_TC=160 \
        -x LD_LIBRARY_PATH -x PATH \
        -mca coll_hcoll_enable 0 -mca pml ob1 -mca btl_tcp_if_include eth0 -mca btl ^openib \
        ./build/all_reduce_perf -b 32M -e 1G -i 1000 -f 2 -g 1
```
## Torch/Megatron-LM
* Because they don't use mpirun, so don't need to set no passward access.
* maybe some evironment don't need set NIC's environment variables too, please check.
* Megatron-LM use torch, mainly Master IP and Master Port need to be set.

