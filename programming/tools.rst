vagrant
================
- Make the box

  go to the virtual box directory (ex: ~/VirtualBox VM/vagrant-precise32)
  then execute

      vagrant package --base vagrant-precise32

  and you'll have the package.box ready to use with vagrant :)

  To add it to your box lists and init

      vagrant box add vagrant-precise32 package.box
      vagrant init vagrant-precise32
      vagrant up

