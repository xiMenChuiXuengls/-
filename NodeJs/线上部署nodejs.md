##### 注册云主机

推荐阿里云主机

#### 部署工作流

以centos7为例:

* 使用xshell客户端进行远程登录
* **安装nodejs**
  * **下载nodejs包**

`wget https://nodejs.org/dist/v10.6.0/node-v10.6.0-linux-x64.tar.xz`

**解压缩包**

```shell
xz -d node-v10.6.0-linux-x64.tar.xz  //后面为下载的安装文件
tar -xf node-v10.6.0-linux-x64.tar  //xz 命令解压后的.tar文件
```

##### 为node和npm创建软连接

先确认nodejs的路径，这里的nodejs路径为~/node-v10.6.0-linux-x64/bin。确认后依次执行 

```shell
ln -s ~/node-v10.6.0-linux-x64/bin/node /usr/bin/node
ln -s ~/node-v10.6.0-linux-x64/bin/npm /usr/bin/npm
```

如果输入`node -v` 和`npm` 能输出版本号 则安装成功



#### 安装方法2——编译安装

**1.安装gcc，make，openssl，wget**

```
yum install -y gcc make gcc-c++ openssl-devel wget1
```

**2.下载源代码包**

在下载页面<https://nodejs.org/en/download/current/>中找到下载地址。然后执行指令

```shell
wget https://nodejs.org/dist/v10.6.0/node-v10.6.0.tar.gz
```

**3.解压源代码包**

```shell
tar -xf node-v10.6.0.tar.gz
```

**4.编译**

进入源代码所在路径

```
cd node-v10.6.0
```

先执行配置脚本

```
./configure
```

编译与部署

```
make && make install
```

接着就是等待编译完成…

**5.测试**

```
node -v
npm
```

如果输出版本号，则部署完成

这种方式安装有点麻烦，还有安装gcc等其他程序。而且编译比较久，切部署完成后nodejs为分别放在好几个文件夹内：

- /usr/local/bin –放置nodejs 执行程序
- /usr/lib –放置了node_modules，即nodejs的各种模块
- /usr/include –放置了nodejs扩展开发用头文件

优点是全局安装nodejs模块，直接使用。

 

#### xz命令 

xz -z 要压缩的文件

如果要保留被压缩的文件加上参数 -k ，如果要设置压缩率加入参数 -0 到 -9调节压缩率。如果不设置，默认压缩等级是6.

xz -d 要解压的文件

同样使用 -k 参数来保留被解压缩的文件。



#### 安装mongodb

用yum方式安装mongodb

1. 创建yum的配置文件  以便用yum直接安装mongodb

   ```shell
   vim /etc/yum.repos.d/mongodb-org-4.0.repo
   ```

   2.按i进入编辑模式 填入以下内容:

   ```shell
   [mongodb-org-4.0]
   name=MongoDB Repository
   baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.0/x86_64/
   gpgcheck=1
   enabled=1
   gpgkey=https://www.mongodb.org/static/pgp/server-4.0.asc
   ```

   按 esc 进入命令模式  输入 `:wq` 保存退出

   3. 安装MongoDB packages

   ```shell
   sudo yum install -y mongodb-org
   sudo yum install -y mongodb-org-4.0.0 mongodb-org-server-4.0.0 mongodb-org-shell-4.0.0 mongodb-org-mongos-4.0.0 mongodb-org-tools-4.0.0
   ```

   **解压文件**

   `tar -zxvf mongodb-linux-*-4.0.0.tgz`

   **开启mongodb服务**

   `sudo service mongod start`

   **设置开机启动**

   `sudo chkconfig mongod on`





#### 安装git

 先卸载自带的git  `yum remove git `

安装git `yum install git `

源码安装git的方式在国内网速较慢 



##### 踩过的坑

使用git clone克隆仓库的时候出现

```
Permissiondenied (publickey).
　　fatal:Could not read from remote repository.
　　Pleasemake sure you have the correct access rights
　　and the repository exists.
```

可能是没有生成密钥

`cd ~/.ssh ls` 可以查看是否有id_rsa和id_rsa.pub文件

没有的话自己生成一个就行

`ssh-keygen -t rsa -C "youremail@example.com" `  youremail@example.com 改为自己的邮箱即可, 一路enter就行 (重新生成的话会覆盖之前的ssh key, 谨慎操作)

`ssh -v git@github.com  `

`ssh-agent -s `

`ssh-add ~/.ssh/id_rsa `

如果出现错误提示: `Could not open a connection to your authentication agent. 

执行 :

```
eval `ssh-agent -s`
后继续执行:
ssh-add ~/.ssh/id_rsa
```

将刚刚生成的id_rsa.pub里面的内容复制到github下 new SSH key中就行.

##### 安装PM2

`npm install pm2 -g `



如果在执行pm2命令的时候 显示 command not found 添加软连接就行

`ln -s /root/www/ProductManager/node_modules/pm2/bin/pm2 /usr/local/bin`

/root/www/ProductManager/node_modules/pm2/bin/pm2为你pm2的安装位置