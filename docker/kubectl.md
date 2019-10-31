# Kubectl

## kubeconfig

kubeconfig 用來紀錄與 k8s cluster 的連線資訊，一般來說文件路徑為 ~/.kube/config

```
$ kubectl config view                       //查看kubeconfig配置
$ kubectl config get-contexts               //查詢有那些cluster
$ kubectl config use-context <CLUSTER NAME> //切換連線的cluster
$ kubectl config current-context            //目前連線的cluster
```

## 資源管理

### namespace
namespace可以讓k8s分隔cluster的資源，避免某個pod佔用太多cpu或memory，namespace有幾個特點
*   一個cluster中的每一個namespace名稱不能重覆
*   namespace被刪除時，裡面的物件也會全部被刪除
*   可以透過 Resource Quotas 限制namespace可存取得資源

```
$ kubectl get namespace                         //查看有那些namespace
NAME          STATUS    AGE
default       Active    23h
kube-public   Active    23h
kube-system   Active    23h

$ kubectl create namespace <NAMESPACE NAME>     //建立namespace
```

### pods
```
$ kubectl get pods --namespace=<NAMESPACE NAME>                         //查看namespace中的pods
$ kubectl get pods --namespace=default
NAME                       READY     STATUS    RESTARTS   AGE
dcardhw-7b85b95966-sck52   1/1       Running   0          23h

$ kubectl describe pods <POD NAME> --namespace=<NAMESPACE NAME>         //查看pod的詳細資料
$ kubectl describe pods dcardhw-7b85b95966-sck52 --namespace=default    //POD NAME新建或重啟會不一樣
$ kubectl delete pods <POD NAME> --namespace=<NAMESPACE NAME>           //刪除pod
$ kubectl logs <POD NAME> --namespace=<NAMESPACE NAME>                  //查看pod logs
```

### services
```
$ kubectl get services --namespace=<SERVICE NAME>                       //查看namespace中的services
$ kubectl get services --namespace=default
NAME              TYPE           CLUSTER-IP   EXTERNAL-IP      PORT(S)        AGE
dcardhw-service   LoadBalancer   10.12.7.16   35.232.227.198   80:30350/TCP   23h
kubernetes        ClusterIP      10.12.0.1    <none>           443/TCP        23h

$ kubectl delete services <SERVICE NAME> --namespace=<NAMESPACE NAME>   //刪除service
```

### 常用指令

#### export
```
$ kubectl get deployment <DEPLOYMENT NAME> -o yaml --export -n=<NAMESPACE NAME>
$ kubectl get service <SERVICE NAME> -o yaml --export -n=<NAMESPACE NAME>
$ kubectl get pod <POD NAME> -o yaml --export -n=<NAMESPACE NAME>
```
