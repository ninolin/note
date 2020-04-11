# 那些變數是 False

## False

```
false_values = [
    False, [], {}, '', 0, 0.0, None
]
for value in false_values:
    print(False if not value else True)
```

## 注意使用None

```
def test_connection():
    return True

def get():
    return []

def get_user_list():
    if not test_connection():
        return None
    else:
        return get()

user_list = get_user_list()
/*
 * 如果是用 if user_list: 就會判斷錯誤，因為[]是False
 */
if user_list is None:
    print('connection error')
else:
    print('user list', user_list)
```