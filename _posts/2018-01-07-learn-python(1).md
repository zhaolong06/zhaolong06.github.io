---
layout: post
title: python入门：数字与字符串
date: 2018-01-07
categories: python
tags: python
---

* content
{:toc}

### 类型转换

```python 
# 整型转换

>>> int(True)
    1

>>> int(False)
    0

>>> int(99.9)
    99

>>> int('99')
    99

# 浮点数转换

>>> float(True)
    1.0

>>> float(False)
    0.0

>>> float('99')
    99.0

>>> float('99.9')
    99.9

# 字符串转换

>>> str(98.9)
    '98.9'

>>> str(1.0e4)
    '10000.0'

>>> str(True)
    'True'
```

### 字符串操作

```python
# 使用+拼接字符串

>>> 'Hello ' + 'World!'
    'Hello World'

# 使用*复制

>>> 'Hello ' * 4
    'Hello Hello Hello Hello '

# 使用[]提取字符

>>> a = 'Hello'
>>> a[0]
    'H'

# 使用[start:end:step]分片
'''
* [:] 提取从开头到结尾的整个字符串
* [start:] 从start提取到结尾
* [:end] 从开头提取到end - 1
* [start:end] 从start提取到end - 1
* [start:end:step] 从start提取到end - 1，每step个字符提取一个
'''  

>>> letters = 'abcdefghijklmnopqrstuvwxyz'
>>> letters[:]
    'abcdefghijklmnopqrstuvwxyz'
    
>>> letters[20:]
    'uvwxyz'

>>> letters[12:15]
    'mno'

>>> letters[-3:]
    'xyz'

>>> letters[18:-3]
    'stuvw'

>>> letters[-6:-2]
    'uvwx'

>>> letters[::7]
    'ahov'

>>> letters[4:20:3]
    'ehknqt'

>>> letters[19::4]
    'tx'

>>> letters[:21:5]
    'afkpu'

# 使用len()获取长度

>>> len(letters)
    26

# 使用split()分割

>>> str = 'Hello World'
>>> str.split(' ')
    ['Hello', 'World']

# 使用join()合并

>>> arr = ['Hello', 'World']
>>> ' '.join(arr)
    'Hello World'

# 去掉首尾不需要的字符

>>> str = '...Hello World...'
>>> str.strip('.')
    'Hello World'

# 将首字母变成大写

>>> str = 'hello world'
>>> str.capitalize()
    'Hello world'

# 将所有字母都变成大写

>>> str = 'hello world'
>>> str.upper()
    'HELLO WORLD'

# 将所有单词的开头字母变成大写

>>> str = 'hello world'
>>> str.title()
    'Hello World'

# 将所有字母转换为小写

>>> str = 'HELLO WORLD'
>>> str.lower()
    'hello world'

# 将所有字母的大小写转换

>>> str = 'Hello World'
>>> str.swapcase()
    'hELLO wORLD'

# 居中

>>> str = 'Hello World'
>>> str.center(20)
    '    Hello World     '

# 左对齐

>>> str = 'Hello World'
>>> str.ljust(20)
    'Hello World         '

# 右对齐

>>> str = 'Hello World'
>>> str.rjust(20)
    '         Hello World'

# 使用replace()替换

>>> str = 'Hello World, Hello World, Hello World'
>>> str.replace('Hello', 'Hi')
    'Hi World, Hi World, Hi World'

>>> str.replace('Hello', 'Hi', 1)  # 最多替换一处
    'Hi World, Hello World, Hello World'

```

