#### k8s 部署 nginx

`nginx-deploy-test.yaml`

使用 Deployment 来部署 nginx 

```yaml
apiVersion: apps/v1
kind: Deployment # 部署控制器
metadata:
  name: nginx-test-deploy
spec:
  replicas: 2 # 副本数量，可以在出故障的时候，自动转移。
  selector:   # 匹配规则，匹配到的，就可以被当前 Deployment 控制。
    matchLabels: # 根据标签来匹配
      app: nginx-cluster
      
  template:   # 模版，使用模版来创建 nginx pod
    metadata:
      labels:
        app: nginx-cluster
    spec:
      containers: # docker 相关设置
      - name: nginx-cluster # 设置容器名称
        image: nginx        # 设置使用的镜像
        ports:
        - containerPort: 80 # 需要暴露的端口
        resources:          # 资源控制
          requests:
            cpu: "1"
            memory: 500Mi
          limits:
            cpu: "2"
            memory: 1024Mi
   

```

---

使用 Service 来暴露接口给外部

`nginx-service-test.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-test-service   
spec:
  selector:  # 匹配规则
    app: nginx-cluster # 当 app = nginx-cluster 时候，应用此规则
  type: NodePort                 
  ports:                       
    - protocol: TCP              
      port: 80     # pod 内部端口            
      targetPort: 80      # k8s 代理层端口，随便设置多少，无所谓      
      nodePort: 30001     # 外部端口 30000 开始设置
```

---

部署

```sh
kubectl apply -f nginx-deploy-test.yaml

kubectl apply -f nginx-service-test.yaml


```

