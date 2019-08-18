---
title: Vagrant 工具介绍
created: '2019-08-17T14:42:37.898Z'
modified: '2019-08-17T15:46:10.754Z'
---

# Vagrant 工具介绍
>Vagrant是一个基于Ruby的工具，用于创建和部署虚拟化开发环境。它 使用Oracle的开源VirtualBox虚拟化系统，使用 Chef创建自动化虚拟环境。
简单的说,就是通过命令行 控制 VirtualBox 虚拟机的创建,实用,删除.

#### Vagrant 简单使用
环境:电脑下载 安装 VirtualBox 和 Vagrant 工具.

获取 Vagrant 支持那些命令参数
``` shell
#vagrant --help

Usage: vagrant [options] <command> [<args>]

    -v, --version                    Print the version and exit.
    -h, --help                       Print this help.

Common commands:
     box             manages boxes: installation, removal, etc.
     cloud           manages everything related to Vagrant Cloud
     destroy         stops and deletes all traces of the vagrant machine
     global-status   outputs status Vagrant environments for this user
     halt            stops the vagrant machine
     help            shows the help for a subcommand
     init            initializes a new Vagrant environment by creating a Vagrantfile
     login           
     package         packages a running vagrant environment into a box
     plugin          manages plugins: install, uninstall, update, etc.
     port            displays information about guest port mappings
     powershell      connects to machine via powershell remoting
     provision       provisions the vagrant machine
     push            deploys code in this environment to a configured destination
     rdp             connects to machine via RDP
     reload          restarts vagrant machine, loads new Vagrantfile configuration
     resume          resume a suspended vagrant machine
     snapshot        manages snapshots: saving, restoring, etc.
     ssh             connects to machine via SSH
     ssh-config      outputs OpenSSH valid configuration to connect to the machine
     status          outputs status of the vagrant machine
     suspend         suspends the machine
     up              starts and provisions the vagrant environment
     upload          upload to machine via communicator
     validate        validates the Vagrantfile
     version         prints current and latest Vagrant version
     winrm           executes commands on a machine via WinRM
     winrm-config    outputs WinRM configuration to connect to the machine

For help on any individual command run `vagrant COMMAND -h`

Additional subcommands are available, but are either more advanced
or not commonly used. To see all subcommands, run the command
`vagrant list-commands`.

```

示例创建虚拟机 centos7
1.创建centos7 目录,进入后,创建执行vagrant init centos/7,获取Vagrantfile文件
```
#mkdir centos7
#cd centos7
#vagrant init centos/7
#ls
Vagrantfile
```
2.查看Vagrantfile,此文件用来描述我们将要创建什么样的Virtual machine.(下面即 Vagrantfile 内容)

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "centos/7"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end

```
3.启动系统,会自动在系统内查找vm box 系统镜像文件,如果查找不到,会自动通过网络下载.此时,通过VirtualBox 就可以看到创建并启动了对应文件.
```
# 启动系统
$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Box 'centos/7' could not be found. Attempting to find and install...
    default: Box Provider: virtualbox
    default: Box Version: >= 0
==> default: Loading metadata for box 'centos/7'
    default: URL: https://vagrantcloud.com/centos/7
==> default: Adding box 'centos/7' (v1905.1) for provider: virtualbox
    default: Downloading: https://vagrantcloud.com/centos/boxes/7/versions/1905.1/providers/virtualbox.box
==> default: Box download is resuming from prior download progress
    default: Download redirected to host: cloud.centos.org
==> default: Successfully added box 'centos/7' (v1905.1) for 'virtualbox'!
==> default: Importing base box 'centos/7'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'centos/7' version '1905.1' is up to date...
==> default: Setting the name of the VM: centos_default_1566052371752_34587
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default: Warning: Connection reset. Retrying...
    default: 
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default: 
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
    default: No guest additions were detected on the base box for this VM! Guest
    default: additions are required for forwarded ports, shared folders, host only
    default: networking, and more. If SSH fails on this machine, please install
    default: the guest additions and repackage the box to continue.
    default: 
    default: This is not an error message; everything may continue to work properly,
    default: in which case you may ignore this message.
==> default: Rsyncing folder: /Users/xl/Documents/centos/ => /vagrant
```
4.进入对应系统,在该目录下,执行对应操作后,即可进入该系统.
经过以上操作后，完成了虚拟机的安装，现在需要登录上虚拟机，进行操作。链接很简单，可以使用第三方（xshell等）shell工具或系统自带的，进行登录
 在系统中，如mac，可直接使用 vagrant ssh 来完成链接。或者使用第三方如xshell，ip地址是：localhost，端口，需要观察，映射的22端口是多少。一般是2200 或者2222
 用户名与密码均是： vagrant
```
#vagrant ssh
[vagrant@bogon ~]$ 
```
5.退出该系统.
```
[vagrant@bogon ~]$ exit
```
6.查看系统状态,获得信息,当前有一台云主机处于运行状态,hostname 是 default.
```
# vagrant status
Current machine states:

default                   running (virtualbox)

The VM is running. To stop this VM, you can run `vagrant halt` to
shut it down forcefully, or you can run `vagrant suspend` to simply
suspend the virtual machine. In either case, to restart it again,
simply run `vagrant up`.
```
7.停止运行虚拟机.
```
#vagrant halt
==> default: Attempting graceful shutdown of VM...
```
8.重新获取状态,状态即为关机.
```
vagrant status
Current machine states:

default                   poweroff (virtualbox)

The VM is powered off. To restart the VM, simply run `vagrant up`
```
9. 删除虚拟机.
```
 vagrant destroy
    default: Are you sure you want to destroy the 'default' VM? [y/N] 
```
10.需要创建其他系统,可以通过 [Vagrant Cloude](https://app.vagrantup.com/ubuntu/boxes/trusty64) 搜索对应系统,获取Vagrantfile文件.将内容放入Vagrantfile文件中.vagrant up即可创建该系统.
### vagrant的命令详解
|命令|作用|
--|--
vagrant box add| 添加box的操作
vagrant init| 初始化box的操作，会生成vagrant的配置文件Vagrantfile
vagrant up| 启动本地环境
vagrant ssh| 通过 ssh 登录本地环境所在虚拟机
vagrant halt| 关闭本地环境
vagrant suspend| 暂停本地环境
vagrant resume| 恢复本地环境
vagrant reload| 修改了 Vagrantfile 后，使之生效（相当于先 halt，再 up）
vagrant destroy| 彻底移除本地环境
vagrant box list| 显示当前已经添加的box列表
vagrant box remove| 删除相应的box
vagrant package| 打包命令，可以把当前的运行的虚拟机环境进行打包
vagrant plugin| 用于安装卸载插件
vagrant status| 获取当前虚拟机的状态
vagrant global-status| 显示当前用户Vagrant的所有环境状态
