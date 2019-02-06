## 前端

编译器 : VSCode

jquery是一个函数库，一个js文件，页面用script标签引入这个js文件就可以使用。

​	1、<http://jquery.com/> 官方网站
​	2、<https://code.jquery.com/> 版本下载	

#### React 文档

React使用文档地址查看： <http://react.css88.com/docs/getting-started.html>

React没有集成ajax功能，要使用ajax功能，可以使用官方推荐的axios.js库来做ajax的交互。 axios库的下载地址：<https://github.com/axios/axios/releases>

#### 安装脚手架工具

```
1、设置npm淘宝景象
npm config set registry https://registry.npm.taobao.org

2、安装
npm install -g create-react-app
```

#### 生成应用项目目录

```
3、生成app
create-react-app my-app

4、启动
cd my-app
npm start

5、生成上线文件
npm run build
```

#### 安装axios模块

```
1、在终端的项目目录，执行如下命令
npm install axios

2、在模块文件中引入
import axios from 'axios';
```