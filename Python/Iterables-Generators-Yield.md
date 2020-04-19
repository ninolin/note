# Iterables-Generators-Yield

## Iterables(可疊代對象)

任可以被 for in 語法去疊代的可以稱為可 Interables，像是list, string等等
```python
s = 'hi'
for i in s:
    print(i)

a = [1, 2]
for i in s:
    print(i)
```

## Generators(生成器)

Generators 也是可疊代對象(Iterables)，但只能疊代一次，生成器不會將所有值存儲在記憶體中，而是即時的產生值

```python
def gen():
    a = [1, 2]
    for i in a:
        yield i
g = gen()
print(g)      
# <generator object gen at 0x10ec37b40>

for j in g:
    print(j)
# 1 
# 2
```

生成器的使用時機會當你需要產生一個很大的List或是不知道可能會多大的List時(檔案目錄)，可能會擔心記憶體不足，就可以使用Generators

```
def big_range(i):
    s = 0
    while s<=i:
        yield s
        s += 1

b = my_range(10000000000000000)
for i in b:
    print(i)
```

使用遞迴取得檔案目錄
```
import os

path = '~/'

def get_file(folder_name):
    for item in os.listdir(folder_name):
        full_path = os.path.join(folder_name, item)
        if os.path.isfile(full_path):
            yield full_path
        elif os.path.isdir(full_path):
            for item in get_file(full_path):
                yield item
            # yield from get_file(full_path)

for file_name in get_file(path):
    print(file_name)
```
