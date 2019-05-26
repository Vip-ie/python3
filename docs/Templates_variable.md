* 模板路径
* 模板变量
* 常用的过滤器
* 静态文件引用
-------------------------------------------------------------------------------
**模板路径**
![Screenshot](img/Templates_url1.jpg)

**模板路径配置**

* 在配置文件setting.py中找到TEMPLATES设置来配置。
* 这是一个设置选项的列表，模板大都包含两项通用设置；两种方式配置模板:

第一种: DIRS 定义一个目录列表，模板引擎按列表顺序搜索这些目录以查找模板源文件。将templates放在主项目目录下。

第二种: APP_DIRS告诉模板引擎是否应该进入每个已安装的应用中查找模板,值为True则模板会去安装了的app下面的templates文件夹查找模板。。所以我们也可以在每个app的里面创建模板目录templates存放模板,这种方式需要将这个app添加到setting.py文件的INSTALLED_APPS列表中。

-------------------------------------------------------------------------------
**模板变量的引入**

![Screenshot](img/Templates_url2.jpg)

**模板变量使用规则**

1. 语法:   {{ 变量名 }}
2. 命名由字母和数字以及下划线组成，不能有空格和标点符号
3. 可以使用字典、模型、方法、函数、列表
4. 不要和python或django关键字重名
5. 变量和查找

**注:**

1. 如果data是一个字典，那么访问data.items将会访问data这个字典的key名为items的值，而不会访问字典的items方法。
2. 点在模板渲染时有特殊的含义。 变量名中点表示查找。 

**模板变量例子**

1. app blog/views.py文件
```
from django.shortcuts import render
from django.http import HttpResponse
from django.template.loader import get_template

def hello():
    return 'django'
class Fruits:
    def __init__(self,name, color):
        self.name = name
        self.colo = color
    def say(self):
        return 'HAHAHAHA'

ap = Fruits('apple','red')
ls = ['x','y','z']
dc = {'a':1,'b':2}

def index(request):
    return HttpResponse('<h1>这是blog主页<h1>')

def index1(request):
    t = get_template('blog/blog_index.html')
    html = t.render({'name':'taka'})
    return HttpResponse(html)

def index2(request):
    return render(request,'blog/blog_index.html',
                  context={
                      'name':'python',     # 字符串
                      'hello':hello,       # 函数
                      'fruits_say':ap.say, # 方法
                      'fruits':ap,         # 类对象
                      'list':ls,           # 列表
                      'dict':dc,           # 字典
                  })
```

2. /templates/blog/blog_index.html文件
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
这个变量是字符串对象 {{ name }}<br>
这个变量是函数对象 {{ hello }}<br>
这个变量是类对象 {{ fruits_say }}<br>
这个变量是类{{ fruits }} <br>
这个变量是列表对象 {{ list }} <br>
这个变量是字典对象 {{ dict }}<br>
</body>
</html>
```

-------------------------------------------------------------------------------
**过滤器**

作用: 对变量进行过滤。在真正渲染出来之前，过滤器会根据功能处理好变量，然后得出结果后再替换掉原来的变量展示出来。

**语法：**`{{fruits|lower}}`

管道符号进行链式调用，比如实现一个功能，先把所有字符变成小写，把第一个字符转换成大写。

**语法：**`{{fruits|lower|capfirst}}`

使用参数:过滤器可以使用参数，在过滤器名称后面使用冒号”:”再加上参数，比如要把一个字符串中所有的空格去掉，则可以使用cut过滤器,

**语法如下: **`{{fruits|cut:" "}}`

**注意：**
使用参数的时候，冒号和参数之间不能有任何空格，一定要紧挨着。

**常用过滤器**

1. add:`字符串相加，数字相加，列表相加，如果失败，将会返回一个空字符串`
2. default:`提供一个默认值，在这个值被django认为是False的时候使用。比如：空字符串、None。区别于default_if_none,这个只有在变量为None的时候才使用默认值。`
3. first:``
4. last:``
5. date:``
6. time:``
7. join:``
8. length:``
9. length_is:``
10. lower:``
11. truncatechars:``
12. truncatewords:``
13. capfirst:``
14. slice:``
15. striptags:``
16. safe:``
17. floatformat:`浮点数格式化`
