
# hardware manage


+ blkid /dev/sda3
+ ls -l /dev/disk/by-uuid

mount /dev/sdb1 /mnt

vim /etc/fstab
```
...
# storage_lvm1

`UUID=f7de34bb-5fa2-4169-b74c-3a7c3b619db4	/myfile		xfs	defaults	1 2`
```

修复ntfs磁盘

`ntfsfix /dev/sdb1`

----

# create keypub

ssh-keygen

ssh-copy-id root@exmop.com

----
# git

```
git branch -a

git pull

git add --all
git add filename

git commit -m "add the file"

git push origin master
```

----

# docker

```
docker run -dit --restart=always --name centos_node1 -p 8280:80 -v /var/www:/storage registry.cn-shenzhen.aliyuncs.com/itcp/centos:latest /usr/sbin/init
--net dobr1 --ip 172.17.0.10
--cpu-shares 170
-m 300m --memory-swap

--privileged
 使用该参数，container内的root拥有真正的root权限。
 否则，container内的root只是外部的一个普通用户权限。
 privileged启动的容器，可以看到很多host上的设备，并且可以执行mount。
 甚至允许你在docker容器中启动docker容器。


自定义网络
yum install -y bridge-utils
docker network create --subnet=172.17.0.0/16 dobr1
brctl show

route  # 或者查看路由

*** --net dobr1 --ip 172.17.0.10   # 指定 ip 为 172.17.0.10 ，连接 dobr1 网桥
```

----

nginx

```
#      add_header Access-Control-Allow-Origin $zzjCorsHost;
#      add_header Access-Control-Allow-Credentials 'true';
#      add_header Access-Control-Allow-Headers X-Requested-With;
#      add_header Access-Control-Allow-Methods GET,POST,OPTIONS;
```
----

-----
