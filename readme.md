<h1 align="center">
  mini-shop-server
</h1>

<h4 align="center">
	构建微信小程序(商城)后端
	<br>🤜基于 Flask框架🤛
</h4>

<div align="center">
  <img alt="img" src="https://ws1.sinaimg.cn/large/006tNbRwly1fx19fcgb2pg308w099kjl.gif" width="40%">
</div>
<div align="center">
  <a href="http://118.25.25.229">线上 API文档</a>
</div>


* 借鉴慕课网的[《Python Flask构建可扩展的RESTful API》](http://coding.imooc.com/class/220.html)的设计模式
* 重构慕课网的[《微信小程序商城构建全栈应用》](https://coding.imooc.com/learn/list/97.html)，源项目基于TP5 + MINA框架
* QQ 交流群 163801325（聊天，斗图，学习，交流。伸手党勿进）

## 亮点
- [自动激活](#auto_active)虚拟环境(autoenv)
- [装饰器的扩展](#extend_decorator)(对第三方库)
- 基于原生的 Flask构建 RESTfull API
- 更灵活的 API文档生成方式(可带 **Token**)
- AOP(面向切面编程)设计，实现 **参数校验层** & **异常统一处理层**
- Ubuntu 16.04上 Nginx + Gunicorn + Pipenv部署


## 目录
- [亮点](#亮点)
- [开发工具](#开发工具)
- [服务器部署](#服务器部署)
- [上传&下载](#上传&下载)
- [骚操作](#骚操作)
-  [三端分离](#三端分离): 后续


## 开发工具
* Python 3.6（虚拟环境：pipenv）
* MySQL
* PyCharm（开发工具）
* Navicat（数据库可视化管理工具）

## 开发环境搭建

### Mysql的安装和数据导入
#### 一、安装
```
$ sudo apt-get install mysql-server
$ sudo apt-get install mysql-client
$ sudo apt-get install libmysqlclient-dev
```
安装过程中，会让你输入密码。<br>
请务必记住密码!<br>
务必记住密码！<br>
记住密码！<br>

查看是否安装成功

```$ sudo netstat -tap | grep mysql```

#### 二、运行
```$ mysql -u root -p```

 **`-u`** 表示选择登陆的用户名，  **`-p`** 表示登陆的用户密码<br>
 上面命令输入之后，会提示输入密码(Enter password)

#### 三、导入
下载 Mysql 数据  [SQL文件](https://github.com/bodanli159951/mini-shop-server/blob/master/zerd.sql)

mysql的每条执行以「分号」结尾
```
mysql> create database zerd; # 建立数据库(zerd)
mysql> use zerd; # 进入该数据库
mysql> source /home/ubuntu/zerd.sql; # 导入某目录下的sql文件
```
导入成功，可以直接查询
```mysql> select * from user;```

删除数据库
```mysql>  drop database zerd;```

### pipenv的安装
如果还未安装pip3包管理工具，请先执行如下语句<br>
```$ sudo apt install python3-pip```

安装 pipenv<br>
```$ pip3 install pipenv```

### 本地启动
```
$ git clone https://github.com/Alimazing/mini-shop-server.git
$ cd mini-shop-server 
$ pipenv --python 3.6 # 指定某 Python版本创建环境
$ pipenv shell # 激活虚拟环境 or 如果没有虚拟环境，则构建新的(默认版本)
$ pipenv install # 安装包依赖
$ python server.py run # 启动方式1:默认5000端口
$ python server.py run -p 8080 # 启动方式2:改为8080端口
$ python server.py run -h 0.0.0.0 -p 8080 # 启动方式3:以本地ip地址访问
```

### 生成临时管理员信息 
```$ python fake.py```

### Pycharm的配置<sup>[[1]](#ref_1)</sup>
Pycharm中 配置 Pipenv生成的虚拟环境，并使用 **`指定端口`** 开启「Debug模式」

1. 获取该虚拟环境下 Python的解释器的路径

<div align="center">
  <img alt="img" src="./media/python_interpreter.jpg" width="80%">
</div>

2. 配置指定端口号
**`Run > Edit Configurations`** <br>
写入 `run -h 0.0.0.0 8080` <br>
等同于，在终端执行 `python server.py run -h 0.0.0.0 -p 8080`

<div align="center">
  <img alt="img" src="./media/debug_configurations.jpg" width="80%">
</div>

3. 开启 Debug
**`Run > Debug 'server'`**

### 其他 pipenv操作
```
$ pipenv install flask # 安装指定模块，并写入到 Pipfile中
$ pipenv install flask==1.0.2 # 安装指定版本的模块
$ pipenv uninstall flask # 卸载指定模块
$ pipenv update flask # 更新指定模块
$ pip list # 查看安装列表
$ pipenv graph # 查看安装列表，及其相应的以来
$ pipenv --venv # 虚拟环境信息
$ pipenv --py # Python解释器信息
$ pipenv –rm # 卸载当前虚拟环境
$ exit # 退出当前虚拟环境
```

## 目录结构
```
| |____app.py
| |____api						# 所有 API接口
| | |______init__.py
| | |____v1						# v1 API接口
| | | |______init__.py
| | | |____token.py				# 令牌
| | | |____user.py				# 用户
| | | |____address.py			# 用户地址
| | | |____banner.py			# 横幅
| | | |____theme.py				# 主题
| | | |____client.py
| | | |____product.py			# 产品
| | | |____category.py			# 分类
| | | |____order.py				#订单
| | |____v2						# v2 API接口
| | | |______init__.py
| | | |____file.py				# 文件处理
| |____validators				# 参数校验层 
| | |______init__.py
| | |____base.py
| | |____params.py
| | |____forms.py
| |____service
| | |______init__.py
| | |____token.py
| | |____order.py
| | |____app_token.py
| | |____user_token.py
|____server.py					# 启动程序
|____fake.py						# 生成临时用户
|____code.md						# 错误码(用于前后端开发)
|____Pipfile						# 包依赖文件
|____.env							# 自动激活虚拟环境
|____.gitignore					# git ignore配置
|____readme.md					# 项目说明文档
```

### 自动生成 api 接口文档
[Swagger](https://swagger.io/) 是一个规范和完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务。

本项目使用 [Flasgger库](https://github.com/rochacbruno/flasgger)自动生成 Swagger风格[(Demo)](https://editor.swagger.io/?_ga=2.211085136.492521077.1539840591-1920768432.1536803925)的API文档。

1、[Swagger Editor](http://editor.swagger.io/) 在网页端直接编辑 API文档

查阅 API文档(本项目)
> 启动服务(DEBUG模式下)<br>
在浏览器端输入：http://localhost:8080/apidocs/#/

## 服务器部署
本项目选择在 Ubuntu 16.04上，用 Nginx + Gunicorn + Pipenv部署<sup>[[3]](#ref_3)</sup>，其中 Gunicorn取代 uWsgi。
> Flask与 uWsgi结合有许多难以处理的 bug

```
gunicorn -w 4 -b 127.0.0.1:8080 shema:app # 在8080端口开启 gunicorn
fuser -k 8080/tcp # 关闭占用8080端口的服务
```

## 上传&下载
### 上传<sup>[[2]](#ref_2)</sup>
具体查看 **`app/api/v2/file.py`** 的 **`upload_file`** 视图函数

### 下载
#### 1. 「静态资源文件」下载

默认下载路径前缀 **`http://0.0.0.0:8080/static/`**
访问 **`app/static/images`** 目录下的资源
```
http://0.0.0.0:8080/static/images/1@theme.png
```
访问 **`app/static/files`** 目录下的资源
```
http://0.0.0.0:8080/static/files/Python面向对象编程指南.epub
```

#### 2. 「数据库内存储的文件」下载
send_from_directory
(占坑)

## 骚操作
### Python相关
#### 1、<span id="auto_active">自动激活<span>
进入项目目录时，自动激活项目所需的虚拟环境

1.1 全局安装

 ```$ pip3 install autoenv```
 
1.2 配置

项目根目录下创建.env文件<br>
写入`pipenv shell`

1.3 将autoenv的激活脚本写入 profile文件中

bash模式

```
$ echo "source `which activate.sh`" >> ~/.bashrc
$ source ~/.bashrc
```
zsh模式

```
$ echo "source `which activate.sh`" >> ~/.zshrc
$ source ~/.zshrc
```

1.4 完成

#### 2、<span id="extend_decorator">对第三方库的装饰器的扩展<span>
具体查看 **`app/lib/redprint.py`** 的 **`doc`** 函数

不改动第三方库 Flasgger的 swag_from(装饰器函数)的源码，对其进行了功能的扩展


## 后续
### 三端分离
#### 1.客户端: mini-shop-wx
基于美团的 [mpvue框架](http://mpvue.com/)开发的微信小程序。（未开始，占坑）

#### 2.服务端: mini-shop-server
基于 Flask框架构建 RESTful API。（正在实现中）

点击查阅 [API文档](http://118.25.25.229/apidocs/#/)(Swagger风格)

#### 3.CMS: mini-shop-cms
基于 Flask框架。（未开始，占坑）

## 参考
【1】<span id="ref_1"></span>[PyCharm配置使用Flask-Script启动以及开启Debug模式](http://www.it610.com/article/4325344.htm)

【2】<span id="ref_2"></span>[Flask 上传文件](https://dormousehole.readthedocs.io/en/latest/patterns/fileuploads.html)

【3】<span id="ref_3"></span>[Flask + Gunicorn + Nginx 部署](https://www.cnblogs.com/Ray-liang/p/4837850.html)

【4】<span id="ref_4"></span>[centos7 下通过nginx+uwsgi部署django应用](http://projectsedu.com/2017/08/15/centos7-%E4%B8%8B%E9%80%9A%E8%BF%87nginx-uwsgi%E9%83%A8%E7%BD%B2django%E5%BA%94%E7%94%A8/)

【5】<span id="ref_5"></span>[Nginx的https配置记录以及http强制跳转到https的方法梳理](https://www.cnblogs.com/kevingrace/p/6187072.html)

【6】<span id="ref_6"></span>[https://blog.csdn.net/cloume/article/details/78252319](https://blog.csdn.net/cloume/article/details/78252319)

