# What's this?

小华仔创建的第一个博客，内容还在不断完善中~~~

## 准备写什么内容?

1，电脑和局域网的一些技术，之前做过这方面的工作。

2，Python相关技术，我目前在学习这方面的知识。

3，前端技术，可能比较渣，搭建这个网页让我学到了一些东西。

4，日常琐碎，让我瞬间顿悟的知识。

5，商业方法应用技巧等等等等。

## 以下是示例图标

- 🚀 小火箭
- ⚡️️ 闪电
- 💎 钻石
- 🔥 燃烧的火焰
- 📼 磁带
- ⏱ 时钟

## 示例代码

一段创建目录代码。。。

```python
import os

#创建目录
def makepath(num_name):
    false_path = []
    for n,m in num_name.items():
        try:
            os.mkdir(rf'C:\Users\Administraotr\Desktop\test\mzitu\{n}')
        #目录创建失败后写入日志
        except:
            false_path.append(n)
            print(rf'{n}目录已被创建')
        else:
            with open(rf'C:\Users\Administrator\Desktop\test\mzitu\{n}\{m}.txt','w') as f:
                f.write(f'{n}{m}')
    return None

if __name__=='__main__':
    num_name = {}
    num = '123'
    name = '456'
	num_name[num] = name
    makepath(num_name)
```



