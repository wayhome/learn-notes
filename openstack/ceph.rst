=======================
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



