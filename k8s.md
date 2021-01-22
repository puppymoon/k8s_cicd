master node
1.etcd
儲存k8s cluster的狀態、配置
2.api server
對外提供api
3.scheduler

4.controller


work node
1.docker
container runtime
2.kubelet
由kubelet去創建container
3.kube-proxy
暴露端口
4.fluentd
node之間的網路通訊

image registry

https://www.katacoda.com/courses/kubernetes/launch-single-node-cluster
https://www.katacoda.com/openshift

查看cluster資訊
``` shell
kubectl cluster-info
```

``` shell
Kubernetes master is running at https://172.17.0.82:8443
KubeDNS is running at https://172.17.0.82:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

查看nodes資訊
``` shell
kubectl get nodes
```

``` shell
NAME       STATUS   ROLES    AGE     VERSION
minikube   Ready    master   6m51s   v1.17.3
```

創建一個pod first-deployment 使用katacoda/docker-http-server
``` shell
kubectl create deployment first-deployment --image=katacoda/docker-http-server
```
``` shell
deployment.apps/first-deployment created
```

查看pods的資訊
``` shell
kubectl get pods
```
``` shell
NAME                               READY   STATUS    RESTARTS   AGE
first-deployment-666c48b44-lg849   1/1     Running   0          106s
```

expose 80 port出來
``` shell
kubectl expose deployment first-deployment --port=80 --type=NodePort
```
``` shell
service/first-deployment exposed
```

設定port為 first-deployment的port
取回host01:$PORT的resource
``` shell
export PORT=$(kubectl get svc first-deployment -o go-template='{{range.spec.ports}}{{if .nodePort}}{{.nodePort}}{{"\n"}}{{end}}{{end}}')
curl host01:$PORT
```
``` shell
<h1>This request was processed by host: first-deployment-666c48b44-lg849</h1>
```

打開dashboard
``` shell
minikube addons enable dashboard
```
``` shell
* The 'dashboard' addon is enabled
```

使用
``` shell /opt/kubernetes-dashboard.yaml作為設定檔
kubectl apply -f /opt/kubernetes-dashboard.yaml
```
``` shell
namespace/kubernetes-dashboard configured
service/kubernetes-dashboard-katacoda created
```

kubernetes-dashboard.yaml
``` shell
apiVersion: v1
kind: Namespace
metadata:
  labels:
    addonmanager.kubernetes.io/mode: Reconcile
    kubernetes.io/minikube-addons: dashboard
  name: kubernetes-dashboard
  selfLink: /api/v1/namespaces/kubernetes-dashboard
spec:
  finalizers:
  - kubernetes
status:
  phase: Active
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: kubernetes-dashboard
  name: kubernetes-dashboard-katacoda
  namespace: kubernetes-dashboard
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 9090
    nodePort: 30000
  selector:
    k8s-app: kubernetes-dashboard
  type: NodePort
```

``` shell

```
``` shell

```
``` shell

```
``` shell

```
``` shell

```
``` shell

```
``` shell

```
