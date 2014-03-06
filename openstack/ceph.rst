z======================
Ceph
=======================
Ceph是统一分布式存储系统,支持三种接口:

-  Object：有原生的API，而且也兼容Swift和S3的API
-  Block：支持精简配置、快照、克隆
-  File：Posix接口，支持快照

案例
=====================
目前Ceph最大的用户案例是Dreamhost的Object Service，目前总容量是3PB，可靠性达到99.99999%

架构
====================

RADOS
-------------------
Ceph的底层是RADOS，它的意思是“A reliable, autonomous, distributed object storage”，
可以通过LIBRADOS直接访问到RADOS的对象存储系统

包含两个组件

- OSD： Object Storage Device，提供存储资源。

- Monitor：维护整个Ceph集群的全局状态。


MDS
-------------------
用于保存CephFS的元数据

RADOS Gateway
-------------------
对外提供REST接口，兼容S3和Swift的API



安装
===============
参考  http://ceph.com/docs/master/start/quick-ceph-deploy/

0. prepare ceph-deploy:

   pip install ceph-deploy

1. Create the cluster:

   ceph-deploy new debian01 debian02 debian03

2. Install Ceph:

   ceph-deploy install debian01 debian02 debian03

3. Add the initial monitor(s) and gather the keys:

   ceph-deploy mon create-initial

4. Add two OSDs. For fast setup, this quick start uses a directory rather than an entire disk per Ceph OSD Daemon. :

   ssh debian02
   sudo mkdir /var/local/osd0
   exit

   ssh debian03
   sudo mkdir /var/local/osd1
   exit

   ceph-deploy osd prepare debian02:/var/local/osd0 debian03:/var/local/osd1
   ceph-deploy osd activate debian02:/var/local/osd0 debian03:/var/local/osd1

5. Use ceph-deploy to copy the configuration file and admin key to your admin node and your Ceph Nodes,
   so that you can use the ceph CLI without having to specify the monitor address and ceph.client.admin.keyring each time you execute a command.

   ceph-deploy admin debian01 debian02 debian03 admin-node

6. Ensure that you have the correct permissions for the ceph.client.admin.keyring.

   sudo chmod +r /etc/ceph/ceph.client.admin.keyring

7. Check your cluster’s health

   ceph health

添加 osd
======================
::

   ssh node1
   sudo mkdir /var/local/osd2
   exit

   ceph-deploy osd prepare node1:/var/local/osd2
   ceph-deploy osd activate node1:/var/local/osd2

Once you have added your new OSD, Ceph will begin rebalancing the cluster by migrating placement groups to your new OSD. You can observe this process with the ceph CLI.

::

  ceph -w


