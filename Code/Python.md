## 多线程应用实例

```python
import threading
import time

class myThread (threading.Thread):
    def __init__(self, threadID, name, counter):
        threading.Thread.__init__(self)
        self.threadID = threadID
        self.name = name
        self.counter = counter
    def run(self):
        print ("开启线程： " + self.name)
        # 获取锁，用于线程同步
        threadLock.acquire()
        print_time(self.name, self.counter, 3)
        # 释放锁，开启下一个线程
        threadLock.release()

def print_time(threadName, delay, counter):
    while counter:
        time.sleep(delay)
        print ("%s: %s" % (threadName, time.ctime(time.time())))
        counter -= 1

threadLock = threading.Lock()
threads = []

# 创建新线程
thread1 = myThread(1, "Thread-1", 1)
thread2 = myThread(2, "Thread-2", 2)

# 开启新线程
thread1.start()
thread2.start()

# 添加线程到线程列表
threads.append(thread1)
threads.append(thread2)

# 等待所有线程完成
for t in threads:
    t.join()
print ("退出主线程")
```

## 多线程和队列结合的实例

```python
from queue import Queue
import time
from threading import Thread

def set_value(q):
    index = 1
    while True:
        q.put(index)
        print('放进去的数据是：', index, '时间：', time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()))
        index += 1
        
def get_value(q):
    while True:
        print('取出来的数据是：', q.get(), '时间：', time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()))
        time.sleep(3)

def main():
    q = Queue(4)
    t1 = Thread(target=set_value, args=[q])
    t2 = Thread(target=get_value, args=[q])

    t1.start()
    t2.start()

if __name__ == '__main__':
    main()
```

