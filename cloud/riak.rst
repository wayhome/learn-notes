安装
=====================
::

  curl http://apt.basho.com/gpg/basho.apt.key | sudo apt-key add -
  sudo bash -c "echo deb http://apt.basho.com $(lsb_release -sc) main > /etc/apt/sources.list.d/basho.list"
  sudo apt-get update
  sudo apt-get install riak-cs
  sudo apt-get install riak

配置
===================

配置 riak
---------------------

更改 ``/etc/riak/app.config`` , 将 **riak_kv** 节的 **storage_backend**  属性替换为如下::

    {add_paths, ["/usr/lib/riak-cs/lib/riak_cs-1.4.4/ebin"]},
    {storage_backend, riak_cs_kv_multi_backend},
    {multi_backend_prefix_list, [{<<"0b:">>, be_blocks}]},
    {multi_backend_default, be_default},
    {multi_backend, [
      {be_default, riak_kv_eleveldb_backend, [
        {max_open_files, 50},
          {data_root, "/var/lib/riak/leveldb"}
      ]},
        {be_blocks, riak_kv_bitcask_backend, [
          {data_root, "/var/lib/riak/bitcask"}
      ]}
    ]},

然后向 **riak_core** 节添加::

    {default_bucket_props, [{allow_mult, true}]},


更改 ``/etc/riak/vm.args``, 修改节点 ip 地址, cookie

配置 riak-cs
------------------------

- 创建管理员

  在 ``/etc/riak-cs/app.config`` 中 设置 {anonymous_user_creation, true}, 管理员创建后可以再禁掉此项.



