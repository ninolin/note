# AWS ECR (Elastic Container Registry)

AWS 的 Container 儲存庫

## 設定IAM及驗證帳號

先在 AWS 的 IAM 建好 USER，同時給與「AmazonEC2ContainerRegistryPowerUser」的權限，取得「存取金鑰 ID」及「私密存取金鑰」後使用 AWS CLI 設定新描述檔
```
$ aws configure set aws_access_key_id <AWS_ACCESS_KEY_ID>
$ aws configure set aws_secret_access_key <AWS_SECRET_ACCESS_KEY>
$ aws configure set default.region <AWS_REGISTRY_REGION>
```

使用get-login做身份驗證
```
$ aws ecr get-login --region <AWS_REGISTRY_REGION> --no-include-email
docker login -u AWS -p <PASSWORD> https://<AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com <==下完get-login的結果，把結果貼上termial執行一次
$ docker login -u AWS -p <PASSWORD> https://<AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
Login Succeeded
```

## 建立儲存庫(Repository)
```
aws ecr create-repository --repository-name <REPO NAME>
```

## 推送 Container Image

推送前要先為 Image 打 Tag
```
$ docker images
REPOSITORY                                                 TAG                 IMAGE ID            CREATED             SIZE
hello_flask                                                latest              3059bfdb8f4a        5 hours ago         893MB

$ docker tag <IMAGE_NAME>:<IMAGE_TAG> <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/<REPOSITORY_NAME>
$ docker tag hello_flask:latest 5566.dkr.ecr.us-east-1.amazonaws.com/nino-test
REPOSITORY                                                 TAG                 IMAGE ID            CREATED             SIZE
5566.dkr.ecr.us-east-1.amazonaws.com/nino-test             latest              3059bfdb8f4a        5 hours ago         893MB
hello_flask                                                latest              3059bfdb8f4a        5 hours ago         893MB
```

推送
```
docker push <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/nino-test
```