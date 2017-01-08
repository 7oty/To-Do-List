To-Do Application
========
to do application devlopmnent for python web framework & Flask

### 环境准备
安装virtulenv
```
pip install virtulenv
或者
sudo apt-get install virtulenv
```
创建虚拟环境和激活虚拟环境
```
virtulenv .venv
source .venv/bin/activate
```
### 代码实例
在config.py中数据库配置
```
MONGODB_SETTINGS = {'DB':'todo'}
```
在app/__init__.py中调用数据库配置
```
from flask_mongoengine import MongoEngine
app.config.from_object('config')
db = MongoEngine(app)
```
在models.py编写todo数据模型
```
from app import db
class Todo(db.Document):
    content = db.StringField()
    time = db.DateTimeField()
    status = db.IntField()
```
### 单元测试
```
import unittest
from app import app
from app.models import Todo
class TodoTestCase(unittest.TestCase):
    def setUp(self):
        print "setUp"

    def tearDown(self):
        print "tearDown"
```

### 部署应用

安装gunicorn和supervisor
```
pip install gunicorn
sudo apt-get install supervisor
```
nginx配置
```
server{
    listen 80;
    location /static{
        alias /home/yourapplication;
    }
    location /{
        proxy_pass http://127.0.0.1:9000;
    }
}
```
配置supervisor
```
[program=todo]
command=/home/yourapplication/venv/bin/gunicorn -b 127.0.0.1:900 run:app
directory=/home/youtapplication/todo
```
