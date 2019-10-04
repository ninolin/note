# 用Virtualenv讓本機可以執行不同版本的Python

## 安裝 Virtualenv

要先安裝Python，至官網下載安裝即可，安裝好後執行下列指令安裝
```
pip3 install virtualenv
```

可以使用pip3 list檢查安裝版本
```
TW-NinoLin:note nino_lin$ pip3 list
Package    Version
---------- -------
pip        19.0.3 
setuptools 40.8.0 
virtualenv 16.7.5 
You are using pip version 19.0.3, however version 19.2.3 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
```

## 在專案下建立虛擬環境

因為想在專案中執行3版的python，使用which python3找到python3的路徑
```
TW-NinoLin:note nino_lin$ which python3
/Library/Frameworks/Python.framework/Versions/3.7/bin/python3
```

專案下建立虛擬環境
```
virtualenv -p <PYTHON3_FOLDER> <PROJECT_FOLDER>
virtualenv -p /Library/Frameworks/Python.framework/Versions/3.7/bin/python3 zion-ip-backend/
```

在專案目錄下執行指令啟動虛擬環境中
```
source /bin/activate
```


