# Install minikube
https://kubernetes.io/docs/tasks/tools/install-minikube/


---


# fk8s lab
```
minikube start --profile fk8s -n 3  /* or add --driver virtualbox */
kubectl config use-context fk8s
```

### node 基本設定
```
kubectl label no fk8s name=fk8s-master-0
kubectl label no fk8s-m02 name=fk8s-node-0
kubectl label no fk8s-m03 name=fk8s-node-1
```

```
kubectl taint nodes fk8s node-role.kubernetes.io/master=:NoSchedule
```

### 建置Namespace
```
kubectl create ns pre-prod
```


---


# hk8s lab
```
minikube start --profile hk8s -n 4  /* or add --driver=virtualbox */
kubectl config use-context hk8s
```

### 新增環境變數
```
kubectl get no -o wide
```

```
vim ~/.bash_profile
export NO_PROXY=節點1_IP,節點2_IP,節點3_IP,節點4_IP
source ~/.bash_profile
```

### node 基本設定
```
kubectl label no hk8s name=hk8s-master-0
kubectl label no hk8s-m02 name=hk8s-node-0
kubectl label no hk8s-m03 name=hk8s-node-1
kubectl label no hk8s-m04 name=hk8s-node-2
```

```
kubectl taint nodes hk8s node-role.kubernetes.io/master=:NoSchedule
```

### work node 3 內部設定
```
minikube ssh -p hk8s -n hk8s-m04
```

```
cd /etc/kubernetes/
sudo mv kubelet.conf ./manifests/
sudo systemctl stop kubelet
sudo systemctl start kubelet
exit
```

### 建置Namespace
```
kubectl create ns production
kubectl create ns development
```

## 修改hk8s/hk8s.yaml
找到default-token-bqvk8字眼
改成secrets 的名字 `kubectl get secrets`


### 建置Deploy,Pod,Service
```
kubectl apply -f hk8s/hk8s.yaml
```


---


# minikube 常用指令

### 清除cluster
```
minikube delete -p fk8s
minikube delete -p hk8s
```

### 暫停運行cluster
```
minikube pause -p fk8s
minikube unpause -p fk8s  /* 啟動 ＊/
```
