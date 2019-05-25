创建一个views.py视图函数
```
from django.http import HttpResponse #导入http包

def test(request):
    return HttpResponse('创建视图函数')
```

**设置路由地址**

打开urls.py文件添加视图函数路由
```
from django.contrib import admin
from django.urls import path
from .views import test   #导入视图函数

urlpatterns = [
    path('admin/', admin.site.urls),
    path('test/',views.test),  #添加视图函数路由
]
```
启动Django项目输入`127.0.0.1:8000/test`访问创建视图函数

**新建app应用**

进入项目目录使用命令新建app`python manage.py startapp blog`