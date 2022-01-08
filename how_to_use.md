[TOC]

# 实践环境
> DISTRIB_ID=Ubuntu
> DISTRIB_RELEASE=20.04
> DISTRIB_CODENAME=focal
> DISTRIB_DESCRIPTION="Ubuntu 20.04.3 LTS"
> Linux ubuntu 5.11.0-44-generic x86_64 GNU/Linux

# 编译yadcc

## 配置软件源

```shell
deb http://ftp.sjtu.edu.cn/ubuntu/ focal main restricted
deb http://ftp.sjtu.edu.cn/ubuntu/ focal-updates main restricted
deb http://ftp.sjtu.edu.cn/ubuntu/ focal universe
deb http://ftp.sjtu.edu.cn/ubuntu/ focal-updates universe
deb http://ftp.sjtu.edu.cn/ubuntu/ focal multiverse
deb http://ftp.sjtu.edu.cn/ubuntu/ focal-updates multiverse
deb http://archive.canonical.com/ubuntu focal partner
deb http://ftp.sjtu.edu.cn/ubuntu/ focal-security main restricted
deb http://ftp.sjtu.edu.cn/ubuntu/ focal-security universe
deb http://ftp.sjtu.edu.cn/ubuntu/ focal-security multiverse
deb http://security.ubuntu.com/ubuntu/ focal-security universe main multiverse restricted
```

## 配置代理

```shell
git config --global https.proxy 'socks5://192.168.5.3:7890'
git config --global http.proxy 'socks5://192.168.5.3:7890'
git config --global http.https://github.com.proxy http://192.168.5.3:7890
git config --global https.https://github.com.proxy https://192.168.5.3:7890
```

## 安装依赖

```shell
sudo apt update
sudo apt upgrade
sudo apt install python3 python git git-lfs autoconf ninja libtool
git clone https://github.com/Tencent/yadcc --recurse-submodules
```

## 编译与安装

```shell
# 编译
./blade build yadcc/...

# 安装
mkdir -p  ~/.yadcc/bin
mkdir -p ~/.yadcc/symlinks
cp build64_release/yadcc/client/yadcc ~/.yadcc/bin/.

ln -s ~/.yadcc/bin/yadcc ~/.yadcc/symlinks/g++
ln -s ~/.yadcc/bin/yadcc ~/.yadcc/symlinks/gcc
ln -s ~/.yadcc/bin/yadcc ~/.yadcc/symlinks/c++
```

# 运行yadcc

```shell
# 查看端口
lsof -i tcp

# 运行daemon
./yadcc-daemon --scheduler_uri=flare://192.168.74.128:8336 --cache_server_uri=flare://192.168.74.128:8337 --token=abc --poor_machine_threshold_processors=0 --servant_priority=dedicated

# 运行shcedule
./yadcc-scheduler --acceptable_tokens=abc --inspect_credential=username:passwd

# 运行cache
./yadcc-cache --acceptable_tokens=abc --inspect_credential=username:passwd

# 运行编译任务
YADCC_LOG_LEVEL=0 YADCC_COMPILE_ON_CLOUD_SIZE_THRESHOLD=0 ~/.yadcc/bin/yadcc g++ ./test.cpp -c
```

# 调试yadcc

```shell
# 查看daemon日志
curl http://localhost:8336/inspect/vars/yadcc
# 查看schedule日志
curl http://young:pass@127.0.0.1:8336/inspect/vars/yadcc
# 查看cache日志
curl http://young:pass@127.0.0.1:8337/inspect/vars/yadcc
```

[yadcc/debugging.md at master · Tencent/yadcc (github.com)](https://github.com/Tencent/yadcc/blob/master/yadcc/doc/debugging.md)
