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
# 一对多查询
**创建views.py视图文件**
```
from django.http import HttpResponse
from .models import Department,Student,Course,Stu_detail

def add_user(request):

    return HttpResponse('数据添加成功')
```
# 一对一查询
# 一对多添加数据
# 多对多添加数据
# 多表查询