
[Kubernetes-部署高可用的MySQL](https://www.kubernetes.org.cn/3985.html)

### 创建ConfigMap
``` bash
$ kubectl create -f ./mysql-configmap.yaml --namespace=kube-public
```

### 创建Services
``` bash
$ kubectl create -f ./mysql-services.yaml --namespace=kube-public
```

### 创建StatefulSet
``` bash
kubectl create -f ./mysql-statefulset.yaml --namespace=kube-public 
```

### 通过执行如下的命令可以查看启动过程：
``` bash
$ kubectl get pods -l app=mysql --watch --namespace=kube-public
```

### MySQL部署环境验证
``` bash
kubectl run mysql-client --image=mysql:5.7 -it --rm --restart=Never -- mysql -h mysql-0.mysql.kube-public
```
``` SQL
CREATE DATABASE demo; 
CREATE TABLE demo.messages (message VARCHAR(250)); 
INSERT INTO demo.messages VALUES ('hello');
```
#### 使用主机名为mysql-read来发送测试请求给服务器：
``` bash
$ kubectl run mysql-client --image=mysql:5.7 -i -t --rm --restart=Never -- mysql -h mysql-read.kube-public
```
``` SQL
SELECT * from demo.messages;
```

### 动态扩容
``` bash
$ kubectl scale statefulset mysql --replicas=2 --namespace=kube-public
```

### 参考资料
《mysql教程》地址：http://www.runoob.com/mysql/mysql-tutorial.html
《Replication》地址：https://dev.mysql.com/doc/refman/5.7/en/replication.html
《Replication Solutions》地址：https://dev.mysql.com/doc/refman/5.7/en/replication-solutions.html
《MySQL Master-Slave Replication on the Same Machine》地址：https://www.toptal.com/mysql/mysql-master-slave-replication-tutorial
《Run a Replicated Stateful Application》地址：https://kubernetes.io/docs/tasks/run-application/run-replicated-stateful-application/

### 作者简介：
季向远，北京神舟航天软件技术有限公司产品经理。本文版权归原作者所有。