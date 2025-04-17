# 0417test
---

## Docker 安裝
```bash
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl 
sudo apt install -y docker.io
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
newgrp docker
```
## Kubectl 安裝
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client --output=yaml
```
## Minikube 安裝與啟動
```bash
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
minikube version
minikube start --driver=docker
```
## Minikube 指令檢查
```bash
kubectl cluster-info
kubectl config view
kubectl get nodes
kubectl get pods
```
## nginx deployment
```bash
kubectl create deployment my-nginx --image=nginx:latest
kubectl expose deployment my-nginx --port=80 --type=LoadBalancer
```
## 專案結構
```bash
test/
├── app.js
├── index.html
├── Dockerfile
├── package.json
```
## app.js
```bash
var express = require('express');
var app = express();
var path = require('path');

app.get('/', function(req, res){
    res.sendFile(path.join(__dirname, 'index.html'));
});

app.listen(3000, function(){
    var host = server.address().address;
    var port = server.address().port;
    console.log("Example app listening at 'http://%s:%s'", host, port);
});
```
## index.html
```bash
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>我的學號</title>
  <style>
    body {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
      background-color: #f5f5f5;
      font-family: Arial, sans-serif;
    }

    .container {
      background: white;
      padding: 40px;
      border-radius: 15px;
      box-shadow: 0 8px 16px rgba(0,0,0,0.2);
      text-align: center;
      width: 90%;
      max-width: 400px;
    }

    h1 {
      margin-bottom: 20px;
    }

    .student-id {
      color: #0077cc;
      font-size: 24px;
      font-weight: bold;
      background: #e0f0ff;
      padding: 10px;
      border-radius: 10px;
    }

    @media (max-width: 600px) {
      .container {
        padding: 20px;
      }

      .student-id {
        font-size: 20px;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>我的學號</h1>
    <div class="student-id">B11007037</div>
  </div>
</body>
</html>
```
## Dockerfile
```bash
FROM node:22.14.0
WORKDIR /app
ADD . /app
RUN npm install
EXPOSE 3000
CMD npm start
```
## package.json
```bash
{
  "name": "test",
  "version": "1.0.0",
  "main": "app.js",
  "scripts": {
    "start": "node app.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "description": "",
  "dependencies": {
    "express": "^5.1.0"
  }
}
```
## push Docker
```bash
cd test/
docker build .
docker images
docker login -u "chia75" -p "********" docker.io
docker tag 77ef1917549a chia75/test
docker push chia75/test
```
