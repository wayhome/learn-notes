安装
===========

数据库
----------

::

 sudo apt-get install mysql-server

 mysql -u root -p

 mysql> create database mogilefs;
 mysql> grant all on mogilefs.* to 'mogile'@'%' identified by 'mogilepw';
 mysql> flush privileges;
 mysql> quit

Tracker
-------------

::

  wget https://mogilefs.googlecode.com/files/mogilefsd_2.67-1~kwick60%2B1_all.deb
  sudo dpkg -i mogilefsd_2.67-1~kwick60+1_all.deb
  sudo apt-get install -f

然后修改 /etc/mogilefs/mogilefsd.conf, 主要是更改里面的数据库配置和上面的一致,
如果想让其他机器也可以访问 tracker,则需修改 listen 为 **listen = 0.0.0.0:7001** 

::

   sudo /etc/init.d/mogilefsd restart

存储节点
----------------------

::

    wget  https://mogilefs.googlecode.com/files/libperlbal-perl_1.80-2~kwick60%2B1_all.deb
    sudo dpkg -i libperlbal-perl_1.80-2~kwick60+1_all.deb
    sudo apt-get install -f
    wget https://mogilefs.googlecode.com/files/mogstored_2.67-1~kwick60%2B1_all.deb
    sudo dpkg -i mogstored_2.67-1~kwick60+1_all.deb


修改/etc/mogilefs/mogstored.conf， 设备和文件将根据 ``docroot`` 的设置存储

::

   sudo /etc/init.d/mogstored restart

Memcached
----------------------
::

    sudo apt-get install memcached

修改 /etc/memcached.conf


Management Tool
---------------------------

::

    wget https://mogilefs.googlecode.com/files/libmogilefs-perl_1.16-1~kwick60%2B1_all.deb 
    sudo dpkg -i libmogilefs-perl_1.16-1~kwick60+1_all.deb
    sudo apt-get install -f
    wget https://mogilefs.googlecode.com/files/mogilefs-utils_2.27-1~kwick60%2B1_all.deb
    sudo dpkg -i mogilefs-utils_2.27-1~kwick60+1_all.deb



::

    mogdbsetup -dbhost=localhost -dbname=mogilefs -dbuser=mogile -dbpassword=mogilepw
