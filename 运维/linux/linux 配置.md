### linux 交换空间(swap space)

#### 什么是 交换空间

当物理内存不足时，使用不活跃的内存页移动到交换空间中

#### 查看交换空间信息

```shell
free -h
swapon --show
```

#### 创建交换空间

一般推荐创建物流内训 1 倍或 1.5 倍的交换空间

```bash
# 创建1GB交换文件
sudo fallocate -l 1G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# 永久生效，添加到/etc/fstab
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

调整交换空间的使用倾向

```bash
# 查看当前值
cat /proc/sys/vm/swappiness

# 临时修改(推荐值10-60)
sudo sysctl vm.swappiness=30

# 永久修改
echo 'vm.swappiness=30' | sudo tee -a /etc/sysctl.conf
```

#### 删除交换空间

```shell
sudo swapoff /swapfile
sudo rm /swapfile
# 并删除/etc/fstab中的对应行
```

                                      