### 名词解释
1) NVM

全称：Node version manager

它是用来进行Node版本管理的，可以通过NVM安装、移除、选择一个Node版本。所以一般需要安装Node环境时，可以先安装NVM。

2) NPM

全称：Node package managernpm

它是用来管理Node包的，可以通过NPM批量安装项目中使用的相关包。每个Node版本会带有一个NPM包管理器。

### NVM安装


下载NVM源码，解压后安装


```
wget https://github.com/cnpm/nvm/archive/v0.23.0.tar.gz
tar -zxvf v0.23.0.tar.gz
cd  nvm-0.23.0/
./install.sh
source ~/.bash_profile
nvm
```


以上步骤即完成了nvm的安装。

### 切换NVM源

```
vim ~/.bashrc
+++
export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node

source ~/.bashrc
```

### 通过NVM安装一个Node版本

```
#(列出所有可安装的版本)
nvm list-remote
#(安装Node-v7.2.0版本)
nvm install v7.2.1
#(列出本地所有可用Node版本)
nvm ls
#(设置当前使用的Node版本，在本地存在多个Node版本时使用)
nvm use v7.2.1
#(设置默认使用的Node版本)
nvm alias default v7.2.1
```

### 切换NPM源

由于国内网络墙的原因，下载包资源时可能会很慢，所以可以把NPM源地址修改为淘宝提供的源：

```
npm config set registry http://registry.npm.taobao.org/
```

必要时也可修改回官方源：

```
npm config set registry https://registry.npmjs.org/
```

### 使用yarn作为包管理器

现在已经不推荐通过npm安装yarn,可以直接通过yum安装.(but it 又会重新安装npm,node.什么鬼)

```
sudo wget https://dl.yarnpkg.com/rpm/yarn.repo -O /etc/yum.repos.d/yarn.repo
sudo yum install yarn
```
