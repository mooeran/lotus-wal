### 前言

lotus-wal是一款基于lotus改版的钱包管理工具，主要有如下改进
1. 可能利用infura进行钱包管理，避免了操作钱包需要搭建链服务器的问题
2. 将部分原来需要多个程序配合执行的命令，集中到一个程序里

### 运行环境
需要ubuntu20.04
* 安装必要组件
```
apt update -y 
apt install wget build-essential libssl-dev curl mesa-common-dev ocl-icd-opencl-dev -y

wget https://www.open-mpi.org/software/hwloc/v1.11/downloads/hwloc-1.11.5.tar.gz && \
tar -xvzpf hwloc-1.11.5.tar.gz && \
cd hwloc-1.11.5 && \
./configure && make && sudo make install 
sudo ldconfig
```
* 下载lotus程序，解压到/bin目录
### 运行lotus-wal
* 申请infura帐号，参考 http://blog.hubwiz.com/2021/05/12/build-filecoin-app-using-infura/
* 获得一个infura的认证key，例如：https://1rv4P0wMAsfySoYBRpqSUzc7xpj:403670587d8122a4cff0597470ec24cf@filecoin.infura.io
* 运行lotus-wal
```sh
lotus daemon --lite --gate-url=https://filecoin.infura.io --gate-auth=1rv4P0wMAsfySoYBRpqSUzc7xpj:403670587d8122a4cff0597470ec24cf
```
* 如果在~/.lotus目录下生成了api和token文件，则说明运行成功，默认的钱包数据都存放在~/.lotus目录下，与原版lotus程序一致


### 命令说明
#### 钱包管理
* 新建钱包
```sh
lotus wallet new bls
```
* 导出钱包私钥
```sh
# 命令会打印私钥，注意保管
lotus wallet export <wallet addr>
```
* 导入钱包私钥
```sh
# 输完命令后，会提示输入私钥
lotus wallet import
```
* 查看钱包列表
```sh
lotus wallet list
```
* 发送fil到指定钱包
```sh
lotus send --from=<send addr> <to addr> <fil amount>
```

#### 其它集成命令
* 更换节点owner钱包
>更换owner钱包需要两个步骤，先用原owner钱包发送命令，然后再用新钱包地址发送命令，如下所示：
```
# 先用原owner地址发送更换owner命令
lotus actor set-owner --really-do-it --actor=<nodeid> <new owner addr> <old owner addr>

# 再用新owner地址发送更换owner命令
lotus actor set-owner --really-do-it --actor=<nodeid> <new owner addr> <new owner addr>

# 执行完成后，立即生效
```
* 从节点钱包提币到owner钱包
```sh
# 提取指定数量的fil到owner钱包
lotus actor withdraw --actor=<nodeid> <fil amount>
```

### 其它说明
* 所有其它命令基本与lotus原版一致，但部分命令在lite模式下不可用
