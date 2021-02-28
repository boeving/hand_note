

```
# 查看资源情况
kubectl get nodes   # rs, services,pods

# 查看命名空间为order的pods具体信息
kubectl get pods -o --namespace=order

# 根据yaml文件配置创建资源
kubectl create -f myrs.yaml

# 强制替换，删除后重新创建资源。会导致服务中断。
kubectl replace --force -f ./pod.json

# 删除资源
kubectl delete node/rs/service

# 查看这个名字的pod日志
kubectl logs podname

# 查看pods结构信息（重点，通过这个看日志分析错误）
# 对控制器和服务，node同样有效
kubectl describe pods xxpodsname --namespace=xxnamespace



```

