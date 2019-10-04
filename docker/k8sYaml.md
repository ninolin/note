# K8s Yaml

## Pod Yaml
```
apiVersion: v1              <=== 指定使用 v1 版本的 api
kind: Pod                   <=== 指定類型為 Pod
metadata:                   <=== 將 Pod 命名為 web，並加入 app:web 的標記
  name: web
  labels:
    app: web
spec:                       <=== 規格描述
  containers:               <=== 描述容器
  - name: frontend          <=== 將容器命名為 frontend
    image: nginx            <=== 使用 nginx 映像檔
    ports:
    - containerPort: 80     <=== 指定使用 80 port
```

## Deployment Yaml

```
apiVersion: extensions/v1beta1  <=== 指定 api 版本的
kind: Deployment                <=== 指定類型為 Deployment
metadata:
  name: web-deployment          <=== 將 Deployment 命名為 web-deployment
  labels:
    app: web-app                <=== 加入 app: web-app 的標記
spec:
  replicas: 3                   <=== 同時建立 3 個 web-app 的 pod
  selector:                     <=== selector 描述了 replicas 的效果要套用在 app: web-app 的 pod 上
    matchLabels:
      app: web-app
  template:                     <=== pod 的 metadata
    metadata:
      labels:
        app: web-app            <=== pod 的 label
    spec:
      containers:
        - name: web-app         <=== container 的 image 位置和 port
          image: asia.gcr.io/k8s-test-254802/dcardhw@sha256:46d976aa8d055685a116eb1af2f07a674c77126400ea818c5f48680d61fd2668
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 8545
              protocol: TCP
```

### Service Yaml
```
apiVersion: v1                  <=== 指定 api 版本的
kind: Service                   <=== 指定類型為 Service
metadata:
  name: web-service             <=== 將 Deployment 命名為 web-service
spec:
  ports:
    - protocol: TCP
      port: 80                  <=== Service 對外的 port
      targetPort: 8545          <=== 此為 Pod 對外開放的 port number
      name: html
  selector:                     <=== 選擇帶有 "app: web-app" 的 pod
    app: web-app
  type: LoadBalancer            <=== type 一共有四種(ClusterIP, NodePort, LoadBalancer, ExternalName), 預設是 ClusterIP
```

