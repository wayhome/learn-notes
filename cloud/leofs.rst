安装
============

erlang
-------------
修改 /etc/apt/sources.list,添加::

   deb http://packages.erlang-solutions.com/debian wheezy contrib

添加 public key::

  curl http://packages.erlang-solutions.com/debian/erlang_solutions.asc | sudo apt-key add -

更新安装::

  sudo apt-get update
  sudo apt-get install erlang
