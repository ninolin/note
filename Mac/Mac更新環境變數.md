# 在Mac上更新環境變數

## 列出全部的環境變數

```
printenv
```

## 列出某環境變數的值

```
echo $env_parameter
```

## 更新環境變數
```
# 打開bash profile
vi ~/.bash_profile

# 新增或修改
export env_parameter=value

# 執行 bash profile
source ~/.bash_profile
```
