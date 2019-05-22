# MacOSX Sublime Text3 Command + B  运行python配置方法

![Screenshot](img/command+b0.jpg)
![Screenshot](img/command+b1.jpg)
![Screenshot](img/command+b2.jpg)
![Screenshot](img/command+b3.jpg)

**python配置shell信息**

```
{
    "cmd":["/usr/local/bin/python3", "-u","$file"],
    "file_regex":"^[ ]*File \"(...*?)\", line([0-9]*)"
}
```