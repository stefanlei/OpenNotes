##### k8s 命令



```sh
# 获取当前上下文中的 namespace
kubectl get ns

# 切换上下文
kubectl config use-context docker-desktop

# 切换命名空间
kubectl config set-context --current --namespace=bi-test


# 发布节点
kubectl create -f test.yaml


# 端口转发
kubectl port-forward
```





##### ks8 dashboard 监控

```sh
# 启动代理
kubectl proxy

# 生成 token
kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep kubernetes-dashboard | awk '{print $1}')


```



