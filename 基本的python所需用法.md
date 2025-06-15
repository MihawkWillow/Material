# 实现NLP的两大步

TRANSORMER

HUNGUNG FACE

## 用于NPL的python的基本用法(正则表达式---指定想要的模式（文本处理和清洗））

### 正则表达式的基本用法

导入re
```
import = re
```
findall
```
input = '自然语言处理很重要。 12abc789'
pattern = re.compile(r'.') 若非转译
re.findall(pattern,input)
```
基本的集合匹配表达
```
pattern = re.compile(r'[a-zA-Z]')集合
pattern = re.compile(r'[^a-zA-Z]')非集合
pattern = re.compile(r'[a-zA-Z]|[0-9]')或
\d数字
\D非数字
\w字母和数字
\W非字母和数字
\s空格
```
重复执行
```
pattern = re.cpmpile(r'\d*')0次或者多次
pattern = re.cpmpile(r'\d+')一次或者多次
pattern = re.cpmpile(r'\d？')0或一次匹配
pattern = re.cpmpile(r'\d{1，3}')0或一次匹配
```
martch and search
返回一个MatchObject
如果匹配不到则返回一个NoneType
```
input = '123自然语言处理'

pattern = re.compile(r'\d')
match = re.match(pattern,input2)
match.group()
```
字符串的替换和修改
```
sub(rule,replace,target[,count])
subn(rule,replace,target[,count])

input = '123自然语言处理'
pattern = re.compiple(r'\d')
re.sub(pattern,'数字'，input2)

返回结果：数字数字数字自然语言处理
如果用subn的话就是会多返回一个替换的次数此次为3
re.sub(pattern,''，input2)直接让替换对象消失

split(re.compile[,maxsplit])
第三个参数是最多分割几次的意思

input3 = '自然语言处理123机器学习456深度学习'
pattern = re.compile(r'\d+')
re.split(pattern,input3)

输出[’自然语言处理‘，’机器学习，’深度学习‘] 
```

命名组
```
（?p...）命名组
<>里面是你给这个组起的名字
```
```
input3 = '自然语言处理123机器学习456深度学习'
pattern = re.compile(r'(?P<dota>\d+)(?P<lol>\D+)')
m = re.search(pattern.input3)
m.group('lol')
m.group('dota')
```
自定义搜索
```
input = 'number 123-456-789'
pattern = re.compile(r'\d\d\d-\d\d\d-\d\d\d')
m = re.search(pattern.input3)
print(m.group())

输出（’123-456-789‘）
```


















> Written with [StackEdit中文版](https://stackedit.cn/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIzMjI3OTE4N119
-->