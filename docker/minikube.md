# Minikube

## Install minikube on mac

首先需安裝virtualbox，再執行下列指令安裝minikube

```
$ brew cask install minikube
$ minikube version
minikube version: v1.4.0
commit: 7969c25a98a018b94ea87d949350f3271e9d64b6
```

## 建立 cluster
```
$ minikube start
$ kubectl cluster-info  //查看cluster情況
$ minikube status       //查看minikube情況
$ minikube dashboard    //UI觀看k8s
```

## minikubu 基本指令
```
$ minikube              //查看minikube基本指令
$ minikube start        //啟動minikube
$ minikube stop         //停止minikube
$ minikube dashboard    //UI觀看k8s
```