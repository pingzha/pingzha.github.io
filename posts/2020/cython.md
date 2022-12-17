---
title: cython
permalink: cython
date: 2020-06-17 21:32:15
tags: cython ctypes
categories: work
---

编写 setup.py 编译脚本

```
from distutils.core import setup
from Cython.Build import cythonize
import commands

cmd = "rm -f ./*.so;rm -rf build;rm -rf ./lib_modules"
(ret,out) = commands.getstatusoutput(cmd)
filelist = ["TaskReblance.py", "test.py"]
setup(
 name="middleware",
 ext_modules=cythonize(filelist)
)
result_path = "./lib_modules/aaa"
cmd = "mkdir -p %s;mv *.so %s" % (result_path, result_path)
(ret,out)=commands.getstatusoutput(cmd)
```

执行编译命令
python setup.py build ext --inplace

测试验证 test.py

import
(ret,out)=commands.getstatusoutput("cd /tmp/")
import TaskReblance
TaskReblance.main()

或直接使用命令：

```python -m TaskReblance.so```

通过cython产生自带main函数的空壳函数
cython test.py --embed
产生的test.c文件通过gcc编译：
gcc -O3 test.c -l/usr/include/python2.7 -lpython2.7
得到可执行文件a.out，将so库和a.out文件放入同一个文件夹中即可

另外用cython代替ctypes功能示意
写一个c功能函数（去掉main函数），通过gcc编译成库后，可以被ctypes直接调用，但还是麻烦，考虑使用cython，增加如下：

```
cdef
  int fib(int)

def fibf(n):
  return fib(n)
```

保存为fibf.pyx文件，然后
cython fibf.pyx (做调用检查，产生fibf.c文件)

```gcc fibf.c -shared -fPIC -l/usr/include/python2.7 -lpython2.7 -o fibf.so -O3```

编译出一个so文件，然后可以直接使用python的import进行导入后使用
