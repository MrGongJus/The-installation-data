### Redis安装

[中文官网](http://www.redis.cn)Redis.cn

·       当前redis最新稳定版本是4.0，常用版本3.2版本。

·       step1:下载

​	wget <http://download.redis.io/releases/redis-3.2.8.tar.gz>

·       step2:解压

​	tar -zxvf redis-3.2.8.tar.gz

·       step3:复制，放到usr/local/redis⽬录下

​	sudo mv ./redis-3.2.8 /usr/local/redis/

·       step4:进⼊redis⽬录

​	cd /usr/local/redis/

·       step5:生成

a)     安装c语言编译器gcc 

​	sudo apt-get install gcc

b)     安装编译命令make   

​	sudo apt-get install make(这一步可能会出问题，根据提示执行命令)

c)      生成 

​	sudo make(比较慢)

·       step6:测试,这段运⾏时间会较⻓

​	sudo make test



如果出现错误,按以下方法安装

解决方法:

cd 

wget http://downloads.sourceforge.net/tcl/tcl8.6.1-src.tar.gz

sudo tar xzvf tcl8.6.1-src.tar.gz  -C /usr/local/

cd  /usr/local/tcl8.6.1/unix/

sudo ./configure

sudo make（时间比较长）

sudo make install .



·       step7:安装,将redis的命令安装到/usr/local/bin/⽬录

​	sudo make install  （时间比较长）

·       step8:安装完成后，我们进入目录/usr/local/bin中查看

​	cd /usr/local/bin

​	ls -all 

​	a)     redis-server      redis服务器

​	b)     redis-cli          redis命令行客户端

​	c)      redis-benchmark  redis性能测试工具

​	d)     redis-check-aof    AOF文件修复工具

​	e)     redis-check-rdb    RDB文件检索工具

​	出现以上文件说明已下载好了.

​	step9:配置⽂件，移动到/etc/⽬录下

·       配置⽂件⽬录为/usr/local/redis/redis.conf

​	sudo cp /usr/local/redis/redis.conf /etc/redis/

## 配置

·       Redis的配置信息在/etc/redis/redis.conf下。

·       查看

​	sudo vi /etc/redis/redis.conf

·       绑定ip：如果需要远程访问，可将此⾏注释，或绑定⼀个真实ip

​	bind 127.0.0.1

·       端⼝，默认为6379

​	port 6379

​	是否以守护进程运⾏

​	daemonize yes

​	数据⽂件

​	dbfilename dump.rdb

​	数据⽂件存储路径

​	dir /var/lib/redis

​	⽇志⽂件

​	logfile /var/log/redis/redis-server.log

​	数据库，默认有16个

​	database 16

​	主从复制，类似于双机备份。

​	slaveof

## 服务器端和客户端命令

·       服务器端的命令为redis-server

·       可以使⽤help查看帮助⽂档

​	redis-server --help

·       个人习惯

​	ps -aux|grep redis 查看redis服务器进程
​	 sudo kill -9 pid 杀死redis服务器
 	sudo redis-server /etc/redis/redis.conf 指定加载的配置文件

​	不推荐使⽤服务的⽅式管理redis服务

·       启动

​	sudo service redis start

·       停⽌

​	sudo service redis stop

·       重启 sudo service redis restart

**客户端**

·       客户端的命令为redis-cli

·       可以使⽤help查看帮助⽂档

​	redis-cli --help

·       连接redis

​	redis-cli

·       运⾏测试命令

​	ping

·       切换数据库

·       数据库没有名称，默认有16个，通过0-15来标识，连接redis默认选择第一个数据库

​	select n	

点击中⽂官⽹查看命令⽂档<http://redis.cn/commands.html>

## 通过go语言和redis数据库进行交互

**安装命令**

go get -u -v github.com/gomodule/redigo/redis

安装完成后，回到家目录创建test.go,把下面代码复制到test.go里面，编译执行test.go，之后在redis中查找到键c1值为hello，说明安装成功

```
package main
import ( "github.com/gomodule/redigo/redis")
func main(){
        conn,_ := redis.Dial("tcp", ":6379")
        defer conn.Close()
        conn.Do("set", "c1", "hello")
}
```

### 操作方法

Go操作redis文档<https://godoc.org/github.com/gomodule/redigo/redis>

**执行数据库操作命令**

```
Send(commandName string, args ...interface{}) error
Flush() error
Receive() (reply interface{}, err error)
```

Send函数发出指令，flush将连接的输出缓冲区刷新到服务器，Receive接收服务器返回的数据

例如：

```
c.Send("SET", "foo", "bar")
c.Send("GET", "foo")
c.Flush()//把缓冲区命令发到服务器
c.Receive() // 接收set请求返回的数据
v, err = c.Receive() // 接收get请求传输的数据
```

**另外一种执行数据库操作命令**

```
Do(commandName string, args ...interface{}) (reply interface{}, err error)
```

**reply helper functions（回复助手函数）**

```
func Scan(src [] interface {},dest ... interface {})([] interface {},error)
```



序列化(字节化)

var buffer bytes.Buffer//容器

enc :=gob.NewEncoder(&buffer)//编码器

err:=enc.Encode(dest)//编码

反序列化（反字节化）

dec := gob.NewDecoder(bytes.NewReader(buffer.bytes()))//解码器

dec.Decode(src)//解码