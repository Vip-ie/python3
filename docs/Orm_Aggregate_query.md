# 聚合函数 Count Avg Max Min Sum, F, Q
views文件内聚合函数查询
```
from django.http import HttpResponse

from .models import User
from django.db.models import Count, Avg, Max, Min, Sum, F, Q
def add_user(request):
    # 求平均年龄
    rs = User.objects.all().aggregate(Avg ('age'))
    
    # 指定一个名字
    rs = User.objects.all().aggregate(average_age = Avg('age'))
    print(rs)

    # 指定一个名字
    # rs = User.objects.all().aggregate(average_age = Avg('age'))
    
    # 最大年龄
    rs1 = User.objects.all().aggregate(Max('age'))
    
    #最小年龄
    rs2 = User.objects.all().aggregate(Min('age'))
    
    #年龄综合
    rs3 = User.objects.all().aggregate(Sum('age'))
    print(rs1,rs2, rs3)

    # 分组查询
    # 每个学员有多少个学生
    Student.objects.filter(department__d_id__isnull=True).update(department_id=5) #更新数据
    rs =Student.objects.values('department')
    rs = Student.objects.values('department').annotate(count=Count('department')).values('department__d_name','count')


    #多对多分组查询
    #查询某个学员报了多少门课
    rs = Student.objects.all().annotate(count=Count('courses')).values('s_name', 'count')

    #某一门课程有多少学生报名
    rs = Course.objects.all().annotate(count=Count('student')).values('c_id','c_name','count')
    print(rs)

    #F查询：针对两个字段的值得比较
    rs = User.objects.all().update(age=F('age')+1)

    # &与 或 | 非 |~
    #| 或
    rs = User.objects.filter(Q(name='xiaoxia') | Q(name='xiaohua'))
    #~ 非
    rs = User.objects.filter(Q(name='xiaoxia') |~ Q(name='xiaohua'))
    #& 与
    rs = User.objects.filter(Q(name='xiaoxia') & Q(name='xiaohua'))
    print(rs)
    return HttpResponse('数据查询成功')

```