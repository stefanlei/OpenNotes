#### k8s Yaml 文件

##### 基础 Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
	name: test
	labels:
		app: nginx-test
spec:
	containers:
		- name: nginx-test
			image: nginx:least
			ports:
				- containerPort: 80
```

---

##### Deploy 

deploy 可以控制 Pod 自动重启等，所以一般创建 deploy

```yaml
apiVersion: apps/v1
kind: Deployment # 部署控制器
metadata:
  name: es-deploy
spec:
  replicas: 1 # 副本数量，可以在出故障的时候，自动转移。
  selector:   # 匹配规则，匹配到的，就可以被当前 Deployment 控制。
    matchLabels: # 根据标签来匹配
      app: es-cluster
      
  template:   # 模版，使用模版来创建  pod
    metadata:
      labels:
        app: es-cluster
    spec:
      initContainers: # 初始容器，可以设置多个，也可以不设置
      - name: "sysctl"
        image: "busybox"
        imagePullPolicy: "Always"
        command: ["sysctl","-w","vm.max_map_count=262144"]
        securityContext:
          privileged: true
      containers: # 主容器相关设置
      - name: es-cluster # 设置容器名称
        image: elasticsearch:7.14.2
        env: # 环境变量设置
        - name: "discovery.type"
          value: "single-node"
        ports:
        - containerPort: 9200 # 需要暴露的端口


```

---

#### service

service 一般用来暴露端口，以供外网访问

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-test-service   
spec:
  selector: # 选择器
    app: web # 匹配到 app=web 的标签
  type: NodePort                 
  ports:                       
    - protocol: TCP              
      port: 80   #  服务暴露在 cluster ip 上的接口，提供给集群内部访问             
      targetPort: 80 # 真正服务的端口
      nodePort: 300001 # 暴露给外网访问的端口


```

---

#### PV

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mypv1 # pv 名称
spec:
  capacity:
    storage: 4Gi # 空间大小
  volumeMode: Filesystem 
  accessModes:
    - ReadWriteOnce # 访问模式
  persistentVolumeReclaimPolicy: Recycle # 回收策略
  storageClassName: hostpath # 自定义类型名称
  hostPath:
    path: /root/pvdata

```

---

#### PVC

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvctest
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: hostpath
```

