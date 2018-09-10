# String



Python的string是不能赋值的，比如，以下程序就会报错。

#### 赋值

```python
s = 'name'
s[0] = 'm'
```

#### Format

```python
s = 'name'
print('Please input {}'.format(s)) # 这里默认是0
>>> Please input name

s1 = 'first name'
s2 = 'last name'
print('{0} {1}'.format(s1, s2)) # 这里默认控制了取后面的数的位置
>>> first name last name
print('{0} {1}'.format(s1, s2))
>>> last name first name

a = 3.1415926
print('{:.2f}'.format(a)) # 控制输出的精度，f表示float
>> 3.14
```

#### 字符的大小型

* str.capitalize\(\)

```python
s = 'name'
>>> 'Name'
```

* 对字符串进行全串检查
  * str.isalpha\(\) : 是否是26个字母
  * str.isdigit\(\) : 是否是数字
  * str.islower\(\) ： 是否小写
  * str.isupper\(\) : 是否大写
  * str.isalnum\(\) : 是否是数字和字母
* str.join\(iterable\) ：拼接

```python
a = ['1','2','3']
'-'.join(a)
>>> '1-2-3'
```

#### str.split\(sep=None, maxsplit=-1\)¶

```python
'1,2,3'.split(',')
>>> ['1', '2', '3']
'1,2,3'.split(',', maxsplit=1)
>>> ['1', '2,3']
'1,2,,3,'.split(',')
>>> ['1', '2', '', '3', '']
```

#### str.strip\(\[chars\]\)

```text
'   spacious   '.strip()
>>>  'spacious'
'www.example.com'.strip('cmowz.')
>>> 'example'
```

基本常用的就这样，其他的不是很常用，遇到了再补充

{% embed data="{\"url\":\"https://docs.python.org/3/library/stdtypes.html\",\"type\":\"link\",\"title\":\"4. Built-in Types — Python 3.7.0 documentation\",\"icon\":{\"type\":\"icon\",\"url\":\"https://docs.python.org/3/\_static/py.png\",\"aspectRatio\":0}}" %}

