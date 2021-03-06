---
title: "Python：more than basic "
date: Feb 27, 2021
---

在看别人代码的过程中，遇到一些在一般的学习中不会遇到的技巧。

```
目录
一、TODO comments
二、Hydra
三、torch contiguous()
四、*args & **args
```



# 一、TODO comments

众所周知，python的注释语句用`#`或```。

在**编辑器中输入注释，注释部分的一些特殊的语句会被高亮**，比如`TODO`，用来表明要做的事情或者未完成的事情。

```python

print("something")

# TODO: finish the structure
```

在Pycharm中，可以直接对带`TODO`的注释统计，以此更方便进行代码完善。

这是[Pycharm官方说明。](https://www.jetbrains.com/help/pycharm/using-todo.html?gclid=CjwKCAiAl4WABhAJEiwATUnEF6UvndUf6vAqR4GbKnus-p7iyJ4caDVgiheeFG0Vr17sujKAkUIahxoCE-IQAvD_BwE)

还有一个类似的：`FIXME`，可以用来表明有bug需要修复。

细节虽小，意义重要。



# 二、Hydra

Hydra是一个开源Python框架，用于简化研究开发与一些复杂应用。

**应用一，自动加载yaml文件。**

示例代码：

config.yaml:

```yaml
db:
  driver: mysql
  user: omry
  pass: secret
```

my_app.py

```python
import hydra
from omegaconf import DictConfig, OmegaConf

@hydra.main(config_name="config")
def my_app(cfg : DictConfig) -> None:
    print(OmegaConf.to_yaml(cfg))

if __name__ == "__main__":
    my_app()
```

然后，config.yaml可以自动加载。

```
$ python my_app.py
db:
  driver: mysql
  pass: secret
  user: omry
```

[点击查看更多的应用例子。](https://hydra.cc/docs/intro/)

# 三、torch contiguous()
在pytorch中，对tensor使用transpose、view等操作后得到的新tensot，与原tensor共享地址。

比如：
```python
x = torch.randn(3,2)
y = torch.transpose(x, 0, 1)
x[0, 0] = 42
print(y[0,0])
# prints 42
```

因此，要想获得不同地址的新tensor，需要调用contiguous()。
```python
y = torch.transpose(x, 0, 1).contiguous()
```

# 四、*args & **args

python中的星号的本质：**unpacking**

一个星号表示对list进行unpack，即：

```python
> list1 = [1,2,3]
> print(list1)
Output: [1,2,3]
> print(*list1)
Output: 1,2,3
```

两个星号表示对dictionary进行unpack，即：

```python
my_first_dict = {"A": 1, "B": 2}
my_second_dict = {"C": 3, "D": 4}
my_merged_dict = {**my_first_dict, **my_second_dict}

print(my_merged_dict)
# Output: {'A': 1, 'B': 2, 'C': 3, 'D': 4}
```

知道了星号的作用，就知道为什么在function的参数里，有时候会用如下形式：

```python
def f(x, *args, **kwargs):
  pass
```

注意，参数是有顺序的，一定是**普通参数、单星号、双星号**。

