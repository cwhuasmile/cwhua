# Queue线程安全队列

1. 初始化Queue(maxsize)：创建一个先进先出的队列。
2. qsize()：返回队列的大小。
3. empty()：判断队列是否为空。
4. full()：判断队列是否满了。
5. get()：从队列中取出最靠前的数据。
6. put()：将一个数据插入到队列的最末尾。

> Queue应用的模式主要是生产者和消费者模式，例如，生产者负责抓取信息，消费者负责解析并存储信息。

# Pyinstaller打包方法

pyinstaller 打包文件
相信很多小伙伴将写的Python代码打包成 .exe文件时使用Python3的Pyinstaller打包工具，下面是pyinstaller的一些参数和命令
pyinstaller -F 文件.py 生成单个可执行文件
pyinstaller -w 文件.py 去掉控制台窗口，对于执行文件没有多大的用处，一般用于GUI面板代码文件
pyinstaller - -icon = 图标路径 表示可执行文件的图标
pyinstaller -c 使用控制台无窗口
pyinstaller -D 生成一个文件夹包括依赖文件
pyinstaller -p 添加Python使用的第三方库
pyinstaller -K 当包含tcl和tk也就是使用tkinter时加上-K参数
例如pyinstaller -F - -icon = 图标文件绝对路径 文件.py
常用的是pyinstaller -F 和pyinstaller -D
————————————————
版权声明：本文为CSDN博主「心寒语录」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/jiyukun1/java/article/details/82218333

# 强制结束线程的方法

python不推荐强制结束线程，原因自行百度。但也不是不可以强制结束，使用ctypes模块可以做到。

```python
import ctypes
import threading
import time

def terminate_thread(thread):
    if not thread.isAlive():
        return
    exc = ctypes.py_object(SystemExit)
    res = ctypes.pythonapi.PyThreadState_SetAsyncExc(
        ctypes.c_long(thread.ident), exc)
    if res == 0:
        raise ValueError("nonexistent thread id")
    elif res > 1:
        ctypes.pythonapi.PyThreadState_SetAsyncExc(thread.ident, None)
        raise SystemError("PyThreadState_SetAsyncExc failed")

def p():
    for i in range(10):
        print(i)
        time.sleep(1)

t = threading.Thread(target=p, args=[])
t.start()
time.sleep(5)
terminate_thread(t)
print("kill已经执行")
time.sleep(7)
```

