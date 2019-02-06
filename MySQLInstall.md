## MySQL安装启动

## 1.1   安装

Linux 下的MySQL安装，十分简单。保证联网状态下，直接使用命令即可顺序完成。

sudo apt-get update

sudo apt-get install mysql-server

会弹出提示，让输入root的密码，根据提示操作即可。

安装完成可以使用 aptitude 命令查看安装是否完成。并能查看到当前所安装的MySQL版本信息。

sudo aptitude show mysql-server

或者也可以使用 mysql -V 查看当前版本信息。

成功安装后，MySQL的配置文件位于 /etc/mysql 目录中.

## 1.2   启动MySQL服务

有两种方法可以启动、关闭、重启MySQL数据库服务器。

方式1：sudo /etc/init.d/mysql start/stop/restart 

方式2：sudo service mysql start/stop/restart

判断是否启动成功，可以使用命令 service mysql status 来查看。

如果看到了一个绿色的小灯亮起，就表示MySQL服务正在欢快地运行着.

​	使用命令：sudo netstat -apn | grep mysql。看到该服务的状态为“LISTENING”，说明服务已经启动，正在等待用户与之建立连接。同时可以看到，mysql服务使用的默认端口为：3306.

## 1.3   登录MySQL数据库

**mysql -h127.0.0.1 -P3306 -uroot -p123456**

如果MySQL服务器在本地，Ip地址可以省略；如果MySQL服务器用的是3306端口，-P也可以省略.

### 设置字符编码

**setnames utf8:** 设置以下编码为utf8.

character_set_client、character_set_database、character_set_results

数据库关闭，设置即失效。如需持久生效则需修改MySQL数据库配置文件
/etc/mysql/mysql.conf.d/mysqld.cnf。

# 1 . Go语言操作MySQL

查询当前主机是否已经安装MySQL驱动：

​	切换到go目录, 查询MYSQL驱动

​	cd go.  

​	find ./ -name "go-sql-driver"

确保ubuntu主机连接网络，输入命令： go getgithub.com/go-sql-driver/mysql.

驱动包中大约包含31个文件。

##  导入MySQL驱动

在Go中操作mysql数据库，需要引入第三方驱动，首先导入以下包名

import
(
   "database/sql"
   _ "github.com/go-sql-driver/mysql"
)

如果不识别_ "github.com/go-sql-driver/mysql"，需要先在github上下载驱动.

##  测试连接数据库

通过调用sql.Open函数得到sql.DB指针;

​	func Open(driverName, dataSourceName string) (*DB, error)

```
defer db.Close()
```

`driverName`: 驱动名。是数据库驱动注册到 database/sql 时所使用的名字。我们*使用”mysql”.

`dataSourceName`: 数据库连接信息，这个连接包含了数据库的用户名, 密码, 数据库主机以及需要连接的数据库名等信息。**语法：**"用户名:密码@[连接方式](主机名**:**端口号)/数据库名".



**真正连接数据库**，需要用**Ping()**方法;

​	func (db *DB) Ping() error



使用 DB 的 Exec() 函数来完成SQL语句在go程序中的执行，访问数据库表。

​	func (db *DB) Exec(query string, args ...interface{}) (Result, error)

返回一条SQL语句的执行结果 Result：

​	type Result interface {

​    		LastInsertId() (int64, error)

​    		RowsAffected() (int64, error)	// RowsAffected() 函数，可以获得成功执行SQL后对数据库所影响的行数。

​	}

### 多行插入

组织SQL语句：

​	sql :=  ` insert into stu values(3, 'rose'), (4, '李白'), (5, '杜甫'); `

使用db.Exec() 函数批量执行SQL语句：

​	result,err:= db.Exec(sql)

使用 result .RowsAffected()函数，显示执行结果影响的行数:

​	n, err := result.RowsAffected()

### 使用DB 的 Prepare()函数，执行带有预处理的SQL语句

### 单行查询

利用QueryRow实现单行查询，能确定该SQL语句的查询结果为一条记录。将结果中的字段值使用Scan()函数依次提取。

使用函数QueryRow，读取数据库表中一条记录。即使写入的sql语句能查询多条结果，该函数也只返回一条记录。

​	func (db *DB) QueryRow(query string, args ...interface{}) *Row

​	row.Scan(&id,&name)	

## 多行查询

​	rows, err := db.Query(sql)

​	for rows.Next() {

​       rows.Scan(&id, &name)

​	}

### 预处理查询

​	stmt,err:= db.Prepare(sql);

​	for rows.Next() {

​       rows.Scan(&id, &name)

​	}