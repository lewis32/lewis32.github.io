---
title: 高阶函数sorted的小坑
date: 2021-06-16 10:56:40
tags:
---

面试中遇到一道题，对形如`x.y.z`的版本号进行排序，面试官没有要求从头写排序算法，第一反应是直接用高阶函数`sorted`进行排序，`cmp`参数传入比较函数，但是执行报错不支持`cmp`参数。因为从`Python3`开始，`sorted`取消了`cmp`参数，需要使用`functools.cmp_to_key`将`cmp`函数转化为`key`函数。

```python
def sort_versions(versions):
	# Python2的写法
    return sorted(versions, cmp=cmp_versions)

def cmp_versions(version1, version2):
    version1, version2 = version1.split('.'), version2.split('.')
    p1, p2 = 0, 0
    n1, n2 = len(version1), len(version2)
    while p1 < n1 or p2 < n2:
        num1 = int(version1[p1]) if p1 < n1 else 0
        num2 = int(version2[p2]) if p2 < n2 else 0
        if num1 > num2:
            return 1
        if num1 < num2:
            return -1
        p1 += 1
        p2 += 1
    return 0

versions = ['1.0.1', '2.0.0', '1', '1.0.0', '1.2.3', '0.1.0', '1.12.0', '1.0.10']
print sort_versions(versions)
```



```python
from functools import cmp_to_key

def sort_versions(versions):
    # Python3的写法
    return sorted(versions, key=cmp_to_key(cmp_versions))

def cmp_versions(version1, version2):
    version1, version2 = version1.split('.'), version2.split('.')
    p1, p2 = 0, 0
    n1, n2 = len(version1), len(version2)
    while p1 < n1 or p2 < n2:
        num1 = int(version1[p1]) if p1 < n1 else 0
        num2 = int(version2[p2]) if p2 < n2 else 0
        if num1 > num2:
            return 1
        if num1 < num2:
            return -1
        p1 += 1
        p2 += 1
    return 0

versions = ['1.0.1', '2.0.0', '1', '1.0.0', '1.2.3', '0.1.0', '1.12.0', '1.0.10']
print(sort_versions(versions))
```

