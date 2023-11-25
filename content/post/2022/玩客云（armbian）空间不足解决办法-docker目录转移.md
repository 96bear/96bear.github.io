---
title: "玩客云（Armbian）空间不足解决办法-docker目录转移"
date: "2022-02-17"
categories: 
  - "linux"
  - "ubuntu"
  - "app"
---

# 玩客云（Armbian）空间不足解决办法-docker目录转移

不用重装，不用恢复，对于我来说把var下的docker占用转移即可 插入u盘，把docker目录转移至此

## 设置静态ip（可选）

这样可以避免每次重启内网ip变动

1. ```shell
    nano /etc/NetworkManager/NetworkManager.conf
    # 禁用network manager
    [ifupdown]
    managed=false
    ```
    
2. ```shell
    nano /etc/network/interfaces
    # 以下按需修改粘贴，内网ip在address
    # Wired adapter #1
    auto eth0
    allow-hotplug eth0
    no-auto-down eth0
    # iface eth0 inet dhcp
    iface eth0 inet static
    hwaddress 12:34:56:78:9A:BC
    address 192.168.1.20
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 192.168.1.1
    ```
    
3. ```shell
    # 重启
    reboot -n 
    ```
    

## 设置u盘自动挂载

1. 插入u盘
    
    ```shell
    # 查看u盘路径/大小/type
    fdisk -l
    # 如/dev/sda4 
    ```
    
2. 格式化u盘为exc4，保持默认，等待完成
    
    ```shell
    # 举例
    mkfs.ext4 /dev/sda4
    ```
    
3. 创建挂载目录
    
    ```shell
    # 举例
    mkdir /mnt/upan
    ```
    
4. 查看u盘UUID
    
    ```shell
    # 举例
    blkid /dev/sda4
    ```
    
5. 修改配置文件，在/etc/fstab后追加
    
    ```shell
    # 例子，uuid和路径改成自己的
    UUID=a63dfbda-29c8-478f-a88e-55796514c961   /mnt/upan/   ext4    defaults    0 0
    ```
    
6. 挂载目录修改权限
    
    ```shell
    chmod -R 777 /mnt/upan/
    ```
    
7. 重启
    
    ```shell
    # 重启
    reboot -n 
    ```
    
8. 检查 挂载目录下存在lost+found目录即为成功

## Docker 修改默认存储路径

1. 在刚刚的挂载目录下创建docker目录
    
    ```shell
    mkdir /mnt/upan/docker
    ```
    
2. 记录原储存路径
    
    ```shell
    docker info|grep "Docker Root Dir:"
    #  Docker Root Dir: /var/lib/docker
    ```
    
3. 修改docker的systemd的 docker.service的配置文件
    
    ```shell
    #查找docker.service的配置文件
    systemctl disable docker
    systemctl enable docker
    #显示结果
    Created symlink /etc/systemd/system/multi-user.target.wants/docker.service → /lib/systemd/system/docker.service. 
    #编辑文件 
    nano /lib/systemd/system/docker.service
    #如何修改(举例)：
    #ExecStart=最后追加--graph=/mnt/upan/docker
    ```
    
4. docker服务重启
    
    ```shell
    systemctl disable docker
    systemctl enable docker
    systemctl daemon-reload
    systemctl restart docker
    ```
    
5. 复制原本的文件到docker新目录,要等一会
    
    ```shell
    # 下面是例子，按2步结果修改cd路径
    cd /var/lib/docker 
    cp ./* /mnt/upan/docker/ -rf 
    ```
    
6. 重启并检查是否成功
    
    ```shell
    systemctl restart docker
    docker ps 
    ```
    
7. 没问题的话删除原目录下文件
    
    ```shell
    rm -rf /var/lib/docker/* 
    ```
    

## 最后看看多出来的空间

```shell
df -hT
```

是不是舒服多了

```shell
# 扩展：查看当前目录文件占用
du -sh *|sort -h     
# output
4.0K    runtimes                                                                                                       
4.0K    swarm                                                                                                         
4.0K    tmp                                                                                                           
4.0K    trust                                                                                                           
20K     builder                                                                                                         
24K     plugins                                                                                                         
80K     network                                                                                                       
112K    buildkit                                                                                                     
224K    volumes
3.0M    image
1.5G    overlay2
2.2G    containers     
```
