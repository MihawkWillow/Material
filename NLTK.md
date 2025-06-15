 ##关于nltk
 安装nltk
 ```
 import nltk #pip install nltk
 nltk.download()
 ```

分词
```
from nlyk.tokensize import word_tokenize
from nlyk.text import Text

input_str = "Today's weather is good, very windy and sunny, we have no classes in the afternoon. We have to play basketball tomorrow."
tokens = word_tokenize(input_str)
tokens = [word.lower() for word in token]
tokens[:5]

输出['today',''','weather','is','good']
```
 帮助文档
```
 help（nltk.text）
 ```
创建文档方便后续操作
```
t = Text(tokens)
t.count('good')数数量
t.index('good')告知下标
t.plot(8)前八个重复最多的词
```
### 具体的操作

停用词
```
from nltk corpus import stopwords
stopwords.readme().replace('\n',' ')
stopwords.fileds()
stopwords.raw('english')看看英语里有哪些停用词
```
过滤掉tokens中的停用词
```
tesy_word = [word.lower() for word in tokens]
test_word_set = set(test_words)
test_word_set.intersection(stopwords.words(''english))
过滤掉停用词
filtered = [w for w in test_word_set if(w not in stopwords.words('english'))] 


```






> Written with [StackEdit中文版](https://stackedit.cn/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk5ODY5NDE0N119
-->