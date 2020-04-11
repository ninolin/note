# List-Tuple-Dictionary-Set

## List(列表)

儲存有順序的資料，由[]包起來，組成元素由逗號隔開

### 建立List
```
list1 = []
list2 = list()
list3 = ['chinese', 'english']
list4 = list('hello')           # ['h', 'e', 'l', 'l', 'o']
```

### 操作List
```
list1 = ['chinese', 'english', 'japanese']
list1[0]    #chinese
list1[1]    #english
list1[-1]   #japanese, -1從結尾回來
```

### List解析
```
a = [1,2,3,4,5,6,7]  # 求偶數

b = []
for item in a:
    if item%2==0:
        b.append(item)
print(b)

[item for item in a if item%2==0]
```


## 字典(Dictionary)

key-value 的資料

### 建立Dictionary

```
dic1 = {'A':'Apple', 'B':'Banana'}
dic2 = dict({'A':'Apple', 'B':'Banana'})
```

### 操作Dictionary

```
dic = {'A':'Apple', 'B':'Banana'}
dic['C'] = 'Cherry'  # 新增
dic['C'] = 'Coconut' # 修改
del dic['C']         # 刪除
dic.keys()           # 全部的key
dic.values()         # 全部的value
dic.items()          # 全部的key-value
```

### List轉換成Dictionary
```
a = [1,2,3,4,5,6,7]
# {1: '1', 2: '2', 3: '3', 4: '4', 5: '5', 6: '6'}
print({item:str(item) for item in a})
```

### Dictionary合併
```
dicA = {'A':'Apple', 'B':'Banana'}
dicB = {'C': 'Cherry'}
{**dicA, **dicB}
```

### 安全取用Dictionary
```
d = {'A':'Apple', 'B':'Banana'}
d['C']  # 存取不存在的key會發生錯誤
```

#### 一般做法用 try-except 或 先判斷
```
try:
    d['C']
except Exception as e:
    print('error for key {}'.format(e))

if 'C' in d:
    print(d['C'])
```

#### 用 get 或 defaultdict
```
d.get('C') # None
d.get('C', 'unknow') # unknow

from collections import defaultdict
d_new = defaultdict(lambda: "unknow", d)
d_new['C']  # unknow
```

## 集合(Set)

一群沒有順序的資料，同時集合中不會有重複的資料

### 建立Set

建立空的集合，不能用{}，{}是Dictionary

```
s1 = set() #
s2 = {1, 2, 3, 4}
```
### 操作Set

新增: add / 刪除: remove

```
s2 = {1, 2, 3, 4}
s2.add(5)
s2.remove(1)
```

常用functoin: len(), sum(), max(), min()

交集(&), 連集(|), 差集(-), 反交集(^)
```
s1 = {3, 4, 5}
s2 = {4, 5, 6, 7}
s1&s2 # {4, 5}
s1|s2 # {3, 4, 5, 6, 7}
s1-s2 # {3}
s2-s1 # {6, 7}
s1^s2 # {3, 6, 7}
```

可以用來判斷字是否在字串中
```
s1 = set("Hello")
print(s1) # {'l', 'H', 'e', 'o'}
print('e' in s1) # True
```

