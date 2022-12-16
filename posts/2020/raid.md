---
title: raid
permalink: raid
date: 2020-06-16 22:29:39
tags: raid lun mdadm
categories: work
---

LVM2软件版本 2.02.180 Release: 8.e17
------命令
组raid5
yes | mdadm --create /dev/md0 --level=5 --raid-devices=4 /dev/sda{5,6,7,8}
查看
cat /proc/mdstat
格式化
mkfs.ext4 /dev/md0
查看
blkid

创建pv
pvcreate /dev/md0
查看
pvscan
详细信息
pvdisplay /dev/md0

创建vg
vgcreate -s 16M vbirdvg /dev/md0
查看vg
vgscan
详细查看
vgdisplay vbridvg

创建LV
lvcreate -L 2G -n vbirdlv vbirdvg
查看
lvscan
lvdisplay /dev/vbirdvg/vbirdlv

扩容/伸缩
放大LV
lvresize -L +500M /dev/vbirdvg/vbirdlv

文件系统更新
resize2fs /dev/vbirdvg/vbirdlv

删除过程与创建相反：
umount
yes | lvremove /dev/vbirdvg/vbirdlv
去使能
vgchange -a n vbirdvg
删除vg
vgremove vbirdvg
pvremove /dev/md0

--------------迁移raid
raid0->raid5
mdadm --create /dev/md0 --level=0 --raid-devices=4 /dev/sda{5,6,7,8}
mdadm --grow /dev/md0 --add /dev/sda9 --level=5
说明：raid0迁移至raid5需要额外的一个热备盘，
      raid5迁移至raid0会踢出一个盘
      raid5->raid6
      raid6->raid5
      raid1->raid5
      raid5->raid1

----------------

发现存储：iscsiadm -m discovery -t st -p xxIP
查看发现记录： iscsiadm -m node
删除发现记录： iscsiadm -m node -o delete -T LUN_NAME -p xxIP
登录存储：iscsiadm -m node -T LUN_NAME -p xxIP -l
登出存储：iscsiadm -m node -T LUN_NAME -p xxIP -u
          iscsiadm -m node -U all
