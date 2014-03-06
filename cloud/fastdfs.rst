安装
==========================
1. 下载

   wget http://jaist.dl.sourceforge.net/project/fastdfs/FastDFS%20Server%20Source%20Code/FastDFS%20Server%20with%20PHP%20Extension%20Source%20Code%20V5.01/FastDFS_v5.01.tar.gz

2. 编译

   sudo apt-get install libevent-dev libpcre3-dev zlib1g-dev
   tar xvf FastDFS_v5.01.tar.gz
   cd FastDFS
   ./make.sh
   sed -i 's/lib64\/libfastcommon.so/lib\/libfastcommon.so/' client/fdfs_link_library.*
   sed -i 's/lib64\/libfdfsclient.so/lib\/libfdfsclient.so/' client/fdfs_link_library.*
   sudo ./make.sh install

3. 更改配置

- /etc/fdfs/tracker.conf 

    base_path=/home/fastdfs #工作目录，记录日志相差信息
    bind_addr=0.0.0.0
    port=22122
    run_by_group=fastdfs
    run_by_user=fastdfs


- /etc/fdfs/storage.conf 

    group_name=group1 #服务器所在组，组内服务器数据同步
    base_path=/home/fastdfs
    store_path0=/home/fastdfs #数据目录
    tracker_server=192.168.1.1:22122 #tracker服务器
    check_file_duplicate=1

- /etc/fdfs/client.conf 

    tracker_server=192.168.1.1:22122


配置说明
====================

tracker配置
----------------------

+------------------------+----------------------------------------------------+-----------------------------------------------------------+----------------------------+
| 名称                   | 描述                                               | 值说明                                                    | 范例                       |
+------------------------+----------------------------------------------------+-----------------------------------------------------------+----------------------------+
| work_threads           | 线程数，通常设置CPU数                              | 正整数值                                                  | work_threads=4             |
+------------------------+----------------------------------------------------+-----------------------------------------------------------+----------------------------+
| store_lookup           | 上传文件的选组方式                                 | 0、1或2。                                                 |                            |
|                        |                                                    | 0：表示轮询                                               |                            |
|                        |                                                    | 1：表示指定组                                             | store_lookup=2             |
|                        |                                                    | 2：表示存储负载均衡（选择剩余空间最大的组）               |                            |
+------------------------+----------------------------------------------------+-----------------------------------------------------------+----------------------------+
| store_group            | 指定上传的组，如果在应用层指定了具体的组,          |                                                           |                            |
|                        | 那么这个参数将不会起效。另外如果store_lookup       |  group1等                                                 | store_group=group1         |
|                        | 如果是0或2，则此参数无效。                         |                                                           |                            |
+------------------------+----------------------------------------------------+-----------------------------------------------------------+----------------------------+
| store_server           | 上传服务器的选择方式                               | 0: 轮询方式（默认）                                       |                            |
|                        |                                                    | 1: 根据ip 地址进行排序选择第一个服务器（IP地址最小者）    |                            |
|                        |                                                    | 2: 根据优先级进行排序（上传优先级由storage server来设置， | store_server=0             |
|                        |                                                    | 参数名为upload_priority），优先级值越小优先级越高。       |                            |
|                        |                                                    |                                                           |                            |
+------------------------+----------------------------------------------------+-----------------------------------------------------------+----------------------------+
| download_server        | 下载服务器的选择方式                               | 同上                                                      | download_server=0          |
+------------------------+----------------------------------------------------+-----------------------------------------------------------+----------------------------+
| reserved_storage_space | 保留空间值。如果某个组中的某个服务器的剩余自由空间 | G or g for gigabyte                                       |                            |
|                        | 小于设定值，则文件不会被上传到这个组。             | M or m for megabyte                                       | reserved_storage_space=1GB |
|                        |                                                    | K or k for kilobyte                                       |                            |
+------------------------+----------------------------------------------------+-----------------------------------------------------------+----------------------------+


storage配置
-------------------------


+----------------------+----------------------------------------------------------+------------------------------+------------------------------------+
| 名称                 | 描述                                                     | 值说明                       | 范例                               |
+----------------------+----------------------------------------------------------+------------------------------+------------------------------------+
| group_name           | 本storage server所属组名                                 |                              | group_name=group1                  |
+----------------------+----------------------------------------------------------+------------------------------+------------------------------------+
| tracker_server       | racker_server 的列表 要写端口号                          |                              | tracker_server=192.168.6.188:22122 |
|                      |                                                          |                              | tracker_server=192.168.6.189:22122 |
|                      |                                                          |                              | tracker_server=192.168.6.190:22122 |
+----------------------+----------------------------------------------------------+------------------------------+------------------------------------+
| upload_priority      | 上传优先级。只有tracker.conf中store_server=2时，才有效。 | 值约小，优先级越高。         |                                    |
|                      |                                                          | 默认为10.                    |  upload_priority=10                |
+----------------------+----------------------------------------------------------+------------------------------+------------------------------------+
| check_file_duplicate | 是否检查file重复。但为1时，使用FastDHT存储文件索引       | 默认为0                      |                                    |
|                      |                                                          | 0, no, false or off：不check | check_file_duplicate=0             |
|                      |                                                          | 1, yes, true or on：check    |                                    |
+----------------------+----------------------------------------------------------+------------------------------+------------------------------------+
|  key_namespace       | 当check_file_duplicate设置为1/trure/on 时,               |                              |                                    |
|                      | 在FastDHT中的命名空间                                    |                              | key_namespace=FastDFS              |
+----------------------+----------------------------------------------------------+------------------------------+------------------------------------+

nginx配置
-----------------------
该模块的配置文件在 fastdfs-nginx-module/src/mod_fastdfs.conf 文件中。具体的配置项解释如下 :

    #连接超时时间，默认值是30秒  
    connect_timeout=2  
  
    #网络超时时间，默认值是30秒  
    network_timeout=30  
  
    #Tracker服务器  
    tracker_server=123.123.123.123:999  
    tracker_server=234.234.234.234:888  
  
    #本机的Storage端口号，默认值为23000  
    storage_server_port=23000  
  
    #本机Storage的组名  
    group_name=group2  
  
    #访问文件的URI是否含有group名称  
    url_have_group_name=true  
  
    #存储路径个数  
    store_path_count=3  
  
    #存储路径  
    store_path0=/data/fastdfs/storage/data  
    store_path1=/data/fastdfs/storage/data  
  
    #日志级别  
    log_level=debug  
  
    #日志名（可选）  
    log_filename=/data/fastdfs/mod_nginx/data  
  
    #当本地不存在该文件时的响应策略，proxy则从其他Storage获取然后响应给client，redirect则将请求转移给其他Storage（HTTP的头设置为本地）  
    response_mode=redirect  
  
    #目前我还未使用过该参数，默认可设置为空  
    if_alias_prefix=  
  
    #是否使用HTTP配置文件，如果使用则前面只留一个#  
    ##include http.conf  

客户端操作
====================
测试上传

   fdfs_test /etc/fdfs/client.conf upload  test.jpg

查看group/storage信息及删除storage：

   fdfs_monitor /etc/fdfs/client.conf
   fdfs_monitor /etc/fdfs/client.conf delete group1 192.168.1.1

删除文件

    fdfs_delete_file /etc/fdfs/client.conf group1/M00/00/00/CgEUylA_dZvx9_4TAAAElz19TBw949.cfg


启用去重
=================
启用去重的话，需要安装fastdht.

安装
------------

    wget http://fastdht.googlecode.com/files/FastDHT_v1.23.tar.gz
    cd FastDHT
    sudo apt-get install libdb-dev
    ./make.sh
    #如果遇到pthread的错误, 将 **if [ -f /usr/lib/libpthread.so ]** 这一整段都删除，只保留 **LIBS="$LIBS -lpthread"** 
    sudo ./make.sh install

配置
--------------

修改 ``/etc/fdht/fdhtd.conf`` , 主要是 base_path. 修改 ``/etc/fdht/fdht_servers.conf``



nginx模块
==================
安装fastdfs-nginx-module模块.

配置 nginx



