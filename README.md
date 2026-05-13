# monitoring

目标： 提供最简和快速的k8s下资源监控交付

当前主服务部署到monitoring namespace下， metrics服务部署到kube-system namespace下


部署：

方法1： Kustomize快速部署

kubectl 从k8s 1.14 版本开始原生支持 Kustomize，无需额外安装独立工具，进入当前目录，执行部署：

```
kubectl create ns monitoring
kubectl apply -k .
```

如果是修改了配置，检查yaml格式输出

```
kubectl kustomize .
```

方法2： 手动部署


一：部署kube-state-metrics 到 系统namespace： kube-system下

```
kubectl apply -f kube-state-metrics.yaml

kubectl -n kube-sysstem get pod | grep kube-state-metrics

```

二：部署node探针和prometheus/grafana到namespace monitoring 下

```
kubectl apply -f node-exporter.yaml -f prometheus.yaml -f grafana-datasources.yaml -f grafana-dashboard.yaml -f grafana.yaml
```

检查：

```
kubectl get pod -n monitoring

NAME                                 READY   STATUS    RESTARTS   AGE
grafana-56c57dbd68-pphcf             1/1     Running   0          157m
node-exporter-6xbbm                  1/1     Running   0          38m
node-exporter-njctb                  1/1     Running   0          38m
node-exporter-zc76s                  1/1     Running   0          38m
prometheus-server-6c9787d6f9-cnmxs   1/1     Running   0          53m
```


三：访问grafana监控页面

```
 kubectl  -n monitoring get svc
NAME                 TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
grafana              LoadBalancer   10.233.38.175   <pending>     3000:30098/TCP   4h15m
prometheus-service   LoadBalancer   10.233.49.123   <pending>     80:30099/TCP     55m

```
grafana默认用户名和密码 admin/admin，首次登录会要求强制修改密码