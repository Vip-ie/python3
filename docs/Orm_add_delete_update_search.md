# 创建数据模型
**创建Django数据模型models.py文件**
```
from django.db import models

#学院表
class Department(models.Model):
    d_id = models.AutoField(primary_key=True)
    d_name = models.CharField(max_length=30)

    def __str__(self):
        return 'Department<d_id=%s,d_name=%s>'%(self.d_id,self.d_name)

#一对多 学生表
class Student(models.Model):
    s_id = models.AutoField(primary_key=True)
    s_name = models.CharField(max_length=30)
    department = models.ForeignKey('Department',on_delete=models.CASCADE,null = True,related_name= 'students')

    def __str__(self):
        return 'Student<s_id=%s,s_name=%s,department_id=%s>'%(self.s_id, self.s_name, self.department_id)

#多对多 课程信息表
class Course(models.Model):
    c_id = models.AutoField(primary_key=True)
    c_name = models.CharField(max_length=30)
    student = models.ManyToManyField('Student', related_name='courses')

    def __str__(self):
        return 'Course<c_id=%s,c_name=%s>'%(self.c_id,self.c_name)

#一对一 学生详情表
class Stu_detail(models.Model):
    student = models.OneToOneField('Student',on_delete=models.CASCADE)
    age = models.IntegerField()
    gender = models.BooleanField(default=1)
    city = models.CharField(max_length=30,null=True)

    def __str__(self):
        return 'Stu_detail<s_id=%s,age=%s,gender=%s,city=%s>'%(self.student_id,self.age,self.gender,self.city)
```
# 插入数据
```
from django.http import HttpResponse
from .models import Department,Student,Course,Stu_detail

def add_user(request):
    # .create()新建数据
    # 插入学院科目分类
    Department.objects.create(d_name='软件')
    Department.objects.create(d_name='设计')
    Department.objects.create(d_name='语言')
    Department.objects.create(d_name='艺术')
    Department.objects.create(d_name='综合')

    # 插入学生信息
    Student.objects.create(s_name='小明',department_id=1)
    Student.objects.create(s_name='小红',department_id=2)
    Student.objects.create(s_name='小花',department_id=3)
    Student.objects.create(s_name='小卡',department_id=4)
    Student.objects.create(s_name='小黑',department_id=5)

    # 插入课程信息
    Course.objects.create(c_name='Python')
    Course.objects.create(c_name='java')
    Course.objects.create(c_name='大数据')
    Course.objects.create(c_name='数据分析')
    Course.objects.create(c_name='养猪')
    Course.objects.create(c_name='摄影')
    return HttpResponse('数据添加成功')
```
# 一对多 查询
**创建views.py视图文件**
```
from django.http import HttpResponse
from .models import Department,Student,Course,Stu_detail

def add_user(request):
    d1 = Department.objects.get(d_id=3) # 一个学院实例对象
    s1 = Student.objects.get(s_id=3)  # 一个学生实例对象
    print(s1.department.d_name)       # 学生所属学院的名称

    默认情况是student_set
    print(d1.student_set.all())        # 反向查询，学院的学生
    # 通过related_name重命名为students,为Student数据模型添加一个重命名related_name=students

    print(d1.students.all()) # 反向查询，学院的学生

    #为指定学生id插入一条学生详细信息
    Stu_detail.objects.create(age=18, gender=0, city='石家庄市', student_id=3)
    std = Stu_detail.objects.get(id=1) #实例化一个学生的详细信息
    print(std) #查询一个学生的详细信息

    return HttpResponse('数据查询成功')
```
# 一对一 查询
```
from django.http import HttpResponse
from .models import Department,Student,Course,Stu_detail

def add_user(request):
    s1 = Student.objects.get(s_id=3) # 一个学生实例对象
    std = Stu_detail.objects.get(id=1) #实例化一个学生的详细信息
    print(std.student.s_name) #正向查询一个学生的名字
    print(s1.stu_detail) #反向查询，直接类名小写

    return HttpResponse('数据查询成功')
```
# 多对多 查询
```
from django.http import HttpResponse
from .models import Department,Student,Course,Stu_detail

def add_user(request):
    s4 = Student.objects.get(s_id=4) # 一个学生实例对象
    c1 = Course.objects.get(c_id=1)  # 一个课程实例对象
    print(c1.student.all())  # 正向查询

    # print(s4.course_set.all()) # 反向查询默认是course_set
    # related_name指定名字courses,为Course数据模型添加一个重命名related_name=courses
    print(s4.courses.all()) 

    d3 = Department.objects.get(d_id=3)
    s3 = Student.objects.get(s_id=3)
    std = Stu_detail.objects.get(id=1)
    c1 = Course.objects.get(c_id=1)
    xh = Student.objects.get(s_id=2)
    print(d1,s1,std,c1,xh)
    return HttpResponse('数据查询成功')
```
# 一对多 添加数据
```
from django.http import HttpResponse
from .models import Department,Student,Course,Stu_detail

def add_user(request):
    d3 = Department.objects.get(d_id=3) #查询科目d_id=3
    xh = Student.objects.get(s_id=2)
    一对多 数据的添加 .add() #添加已经存在的数据
    d3.students.add(xh)
    更新数据添加数据
    xiaohei = Student.objects.get(s_id=5)
    d3.students.add(xiaohei)

    新建数据 .create()
    d3.students.create(s_name='汪洋')
    d3.students.create(s_name='baidu')
    return HttpResponse('数据添加成功')
```
# 多对多 添加数据
```
from django.http import HttpResponse
from .models import Department,Student,Course,Stu_detail

def add_user(request):
    c1 = Course.objects.get(c_id=1)
    c2 = Course.objects.get(c_id=2) #查询课程c_id=2的课程 实例化
    s3 = Student.objects.get(s_id=3) #查询s_id=3 的学生实例化
    s3.courses.add(c1,c2)  #多对多插入课程和科目
    s3.courses.create(c_name='php') #插入新的课程数据到s3学生
    return HttpResponse('数据添加成功')
```
# 多表查询
```
from django.http import HttpResponse
from .models import Department,Student,Course,Stu_detail

def add_user(request):
    # 查询学院名字为"语言"的学生信息
    rs = Student.objects.filter(department__d_name='语言')
    # 查询学生名字中包含'小'的学生的学院信息
    rs = Department.objects.filter(students__s_name__contains='小')
    # 查询学号为3的所有学生课程
    rs = Course.objects.filter(student__s_id=3)
    # 查询报了课程1的所有学生
    rs =Student.objects.filter(courses__c_id=1)
    #查询报了'python'课程的学生的所属学院的信息
    rs = Department.objects.filter(students__courses__c_name='python')
    print(rs)
    return HttpResponse('数据查询成功')
```
# 一对多 .remove()删除
必须保证外键列允许为空
```
from django.http import HttpResponse
from .models import Department,Student,Course,Stu_detail

def add_user(request):
    xh = Student.objects.get(s_id=2)
    d3 = Department.objects.get(d_id=3) #查询科目d_id=3
    d3.students.remove(xh)
    return HttpResponse('数据删除成功')
```
# 多对多 .remove()删除
删掉的是中间信息表信息
```
from django.http import HttpResponse
from .models import Department,Student,Course,Stu_detail

def add_user(request):
    c1 = Course.objects.get(c_id=1)
    c2 = Course.objects.get(c_id=2)
    d3 = Department.objects.get(d_id=3) #查询科目d_id=3
    s3 = Student.objects.get(s_id=3) #查询s_id=3 的学生实例化
    s3.courses.remove(c1,c2)
    print(s3.courses.all()) # 查看报的课程
    print(s3.courses.clear()) # 清空所有报的课程
    return HttpResponse('数据删除成功')
```