## #FastDFS安装

###### 3.2.5.1安装FastDFS依赖包

1. 解压缩libfastcommon-master.zip
2. 进入到libfastcommon-master的目录中
3. 执行**./make.sh**
4. 执行**sudo ./make.sh install**

###### 3.2.5.2安装FastDFS

1. 解压缩fastdfs-master.zip

2. 进入到 fastdfs-master目录中

3. 执行 **./make.sh**

4. 执行 **sudo ./make.sh install**

   3.2.5.3配置跟踪服务器tracker

   1. ```shell
      sudo cp /etc/fdfs/tracker.conf.sample /etc/fdfs/tracker.conf
      ```

   2. 在/home/itcast/目录中创建目录 fastdfs/tracker      

      ```shell
      mkdir -p /home/itcast/fastdfs/tracker
      ```

   3. 编辑/etc/fdfs/tracker.conf配置文件    sudo vim /etc/fdfs/tracker.conf

   ​        修改 base_path=/home/itcast/fastdfs/tracker

###### 3.2.5.4配置存储服务器storage 

1. ```
   sudo cp /etc/fdfs/storage.conf.sample /etc/fdfs/storage.conf
   ```

2. 在/home/itcast/fastdfs/ 目录中创建目录 storage

   ```shell
   mkdir –p /home/itcast/fastdfs/storage
   ```

3. 编辑/etc/fdfs/storage.conf配置文件  sudo vim /etc/fdfs/storage.conf

   修改内容：

   ```shell
   base_path=/home/itcast/fastdfs/storage
   store_path0=/home/itcast/fastdfs/storage
   tracker_server=自己ubuntu虚拟机的ip地址:22122
   ```

   ###### 3.2.5.5启动tracker和storage

   进入到/etc/fdfs/下面执行以下两条指令

   ```shell
   sudo  fdfs_trackerd  /etc/fdfs/tracker.conf
   sudo fdfs_storaged  /etc/fdfs/storage.conf
   ```

   ###### 3.2.5.6测试是否安装成功

   1. **sudo cp /etc/fdfs/client.conf.sample /etc/fdfs/client.conf **
   2. 编辑/etc/fdfs/client.conf配置文件  **sudo vim /etc/fdfs/client.conf**

   修改内容：

   ```shell
   base_path=/home/itcast/fastdfs/tracker
   tracker_server=自己ubuntu虚拟机的ip地址:22122
   ```

   3.上传文件测试(fastDHT)

   sudo fdfs_upload_file /etc/fdfs/client.conf 要上传的图片文件 

   如果返回类似**group1/M00/00/00/rBIK6VcaP0aARXXvAAHrUgHEviQ394.jpg **的文件id则说明文件上传成功

###### 3.2.5.7安装fastdfs-nginx-module 

1. 解压缩 nginx-1.8.1.tar.gz

2. 解压缩 fastdfs-nginx-module-master.zip

3. 进入nginx-1.8.1目录中

4. 执行

   ```shell
   sudo ./configure  --prefix=/usr/local/nginx/ --add-module=fastdfs-nginx-module-master解压后的目录的绝对路径/src
   ```

   注意：**这时候会报一个错，说没有PCRE库**

下载缺少的库

```shell
sudo apt-get install libpcre3 libpcre3-dev 
```

- 首先你需要去更换源，因为ubuntu自带的源没有这个库

- 更换下载源为阿里的源

- 先把原来的源文件备份

  sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

编辑源文件

```shell
sudo vim /etc/apt/sources.list
```

把原来的内容全部删掉，粘贴一下内容：

deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
sudo
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse

更换完源之后执行:

sudo apt-get  update
sudo apt-get install libpcre3 libpcre3-dev

然后进入nginx-1.8.1目录中，再次执行：

sudo ./configure  --prefix=/usr/local/nginx/ --add-module=fastdfs-nginx-module-master解压后的目录的绝对路径/src

然后编译：

​	sudo make

这时候还会报一个错

解决方法：

找到objs目录下的Makefile

vim Makefile

删掉里面的-Werror(**如果没有修改权限，修改一下这个文件的权限,`chmod 777 Makefile`**)

![1538185926173](/Users/gonggongtiansi/Downloads/day07/1-%E4%B8%8A%E8%AF%BE%E8%B5%84%E6%96%99/./assets/1538185926173.png)

然后回到nginx-1.8.1目录中

执行完成后继续执行**sudo make**

执行**sudo make install** 

5.sudo cp fastdfs-nginx-module-master解压后的目录中src下mod_fastdfs.conf   /etc/fdfs/mod_fastdfs.conf

6.sudo vim /etc/fdfs/mod_fastdfs.conf修改内容：

​	connect_timeout=10
​	tracker_server=自己ubuntu虚拟机的ip地址:22122
​	url_have_group_name=true
​	store_path0=/home/itcast/fastdfs/storage

7.sudo cp 解压缩的fastdfs-master目录中的conf中的http.conf  /etc/fdfs/http.conf

8.sudo cp 解压缩的fastdfs-master目录中conf的mime.types /etc/fdfs/mime.types

9.sudo vim /usr/local/nginx/conf/nginx.conf

在http部分中添加配置信息如下：

server {
	listen       8888;
	server_name  localhost;
	location ~/group[0-9]/ {
	ngx_fastdfs_module;
	}
	error_page   500 502 503 504  /50x.html;
	location = /50x.html {
	root   html;
	}
}

10.启动nginx

sudo  /usr/local/nginx/sbin/nginx

这时候会报一个错：

这是因为我们的网络有防火墙，不能直接去google下载相应的包，所以就失败

解决办法:

在`~/workspace/go/src`目录下面创建一个golang.org/x目录

​	cd  ~/workspace/go/src
​	mkdir -p golang.org/x

进入golang.org/x下载两个包

​	cd golang.org/x
​	git clone https://github.com/golang/crypto.git
​	git clone https://github.com/golang/sys.git

然后再执行最初的下载命令

go get github.com/weilaihui/fdfs_client



# go操作fastDFS的方法

先导包，把我们下载的包导入

​	import "github.com/weilaihui/fdfs_client"

导包之后,我们需要指定配置文件生成客户端对象

​	client,_:=fdfs_client.NewFdfsClient("/etc/fdfs/client.conf")

通过文件名上传**UploadByFilename **,参数是文件名（必须通过文件名能找到要上传的文件），返回值是fastDFS定义的一个结构体，包含组名和文件ID两部分内容

​	fdfsresponse,err := client.UploadByFilename("flieName")

通过字节流上传**UploadByBuffer**,参数是字节数组和文件后缀，返回值和通过文件名上传一样。

​	fdfsresponse,err := client.UploadByBuffer(fileBuffer,ext)