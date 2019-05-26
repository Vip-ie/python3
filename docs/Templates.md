**模板放在哪?**

* 1.在主目录下创建一个templates目录用来存放所有的html的模板文件.
* 2.templates目录里面在新建各个以app名字命名的目录来存放各个app中模板文件.

![Screenshot](img/Templates1.jpg)
![Screenshot](img/Templates2.jpg)

**模板渲染实例**

在app目录views.py文件编写渲染代码
```
from django.shortcuts import render
from django.http import HttpResponse
from django.template.loader import get_template

def index(request):
    return HttpResponse('<h1>这是blog主页<h1>')
#index1方法渲染模板
def index1(request):
    t = get_template('blog/blog_index.html')
    html = t.render({'name':'taka'})
    return HttpResponse(html)
#index2方法渲染模板
def index2(request):
    return render(request,'blog/blog_index.html',context={'name':'taka'})
#context是传参数内容的方法
```

**在app目录urls.py文件内编写路由**
```
from django.urls import path
from . import views

urlpatterns = [
    path('/index/',views.index),
    path('/index1/',views.index1),
    path('/index2/',views.index2),
    ]
```

**在templates目录下建立blog目录并建立blog_index.html文件**
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
这是blog主页 {{ name }}
</body>
</html>

```