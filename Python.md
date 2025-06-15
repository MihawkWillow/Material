## 列表推导式
### 例 1：将一个数字列表平方
```
nums = [1, 2, 3, 4]
squares = [x**2 for x in nums]

equals

squares = []
for x in nums:
    squares.append(x**2)
```

例 2：筛选偶数
```
nums = [1, 2, 3, 4, 5, 6]
evens = [x for x in nums if x % 2 == 0]

equals

evens = []
for x in nums:
    if x % 2 == 0:
        evens.append(x)


```


> Written with [StackEdit中文版](https://stackedit.cn/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NzMxNjU3NTldfQ==
-->