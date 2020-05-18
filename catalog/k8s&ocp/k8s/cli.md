#### **K8s cli - kubectl**

1. create resource  # 创建一个资源从一个文件或标准输入
	 ```
   kubectl create deployment nginx --image=nginx:1.14 
   kubectl create -f my-nginx.yaml
   ```
2. run specific image  # 在集群中运行一个指定的镜像
   ```
   kubectl run nginx -it --rm=true --image=nginx --command bash
   kubectl run busybox -it --rm=true --image=busybox --restart=Never
   ```
3. expose service# 创建Service对象以将应用程序"暴露"于网络中
   ```
   kubectl expose deployment/nginx  --type="NodePort" --port=80 --name=nginx
   ```
4. get resource  # 显示一个或更多resources资源
   ```
   kubectl get cs  # 查看集群状态
   kubectl get nodes  # 查看集群节点信息
   kubectl get ns     # 查看集群命名空间
   kubectl get svc -n kube-system  # 查看指定命名空间的服务
   kubectl get pod <pod-name> -o wide  # 查看Pod详细信息
   kubectl get pod <pod-name> -o yaml  # 以yaml格式查看Pod详细信息
   kubectl get pods  # 查看资源对象，查看所有Pod列表
   kubectl get rc,service  # 查看资源对象，查看rc和service列表
   kubectl get pod,svc,ep --show-labels  # 查看pod,svc,ep能及标签信息
   kubectl get all --all-namespaces      # 查看所有命名空间下的所有资源
   ```
5. check cluster information  # 查看集群状态信息
	 ```
   kubectl cluster-info
   ```
6. describe resource  # 描述资源对象
	 ```
   kubectl describe nodes <node-name>  # 显示Node的详细信息
   kubectl describe pods/<pod-name>    # 显示Pod的详细信息
   ```
7. kubectl scale pod  # 扩容与缩容
	 ```
   kubectl scale deployment nginx --replicas 5  # 扩容
   kubectl scale deployment nginx --replicas 3  # 缩容
   ```
8. show api-resources # 查看服务器上支持的API资源
   ```
   kubectl api-resources
   ```
9. generate yaml of resource
   ```
   kubectl create deploy nginx-dp --image=nginx -o yaml --dry-run > deploy.yaml  # dry-run生成yaml文件
   kubectl get deploy nginx-dp -o yaml --export > deploy.yaml                    # get命令导出yaml文件
   kubectl explain pods.spec.containers                                          # desc the fields
   ```
