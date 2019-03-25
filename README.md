# learn-centos7
###### CentOS 7 调整 home分区 扩大 root分区
https://blog.csdn.net/qq_38046109/article/details/71125081


1.查看分区
df -h (centos-home和centos-root每人的名字可能不一样) 
vgdisplay (查看空闲磁盘大小）

2.备份home分区文件
tar cvf /tmp/home.tar /home

3.卸载/home，如果无法卸载，先终止使用/home文件系统的进程
umount /home （卸载）

fuser -km /home/（终止）

4.删除/home所在的lv
lvremove /dev/mapper/centos-home

5.扩展/root所在的lv
lvextend -L +50G /dev/mapper/centos-root

6.扩展/root文件系统
xfs_growfs /dev/mapper/centos-root

7.重新创建home lv 
lvcreate -L 50G -n /dev/mapper/centos-home

8.创建文件系统
mkfs.xfs /dev/mapper/centos-home

9.挂载home
mount /dev/mapper/centos-home

10.home文件恢复
tar xvf /tmp/home.tar -C /home/

