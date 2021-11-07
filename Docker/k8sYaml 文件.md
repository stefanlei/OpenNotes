#### k8s Yaml 文件

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

