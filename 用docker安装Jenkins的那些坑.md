### 用docker安装Jenkins的那些坑

镜像推荐：jenkins/jenkins:lts	或者	jenkinsci/blueocean



```
docker run -u root --rm -d -p 8080:8080 -p 50000:50000 \
-v ~/jenkins:/var/jenkins_home \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /usr/bin/docker:/usr/bin/docker \
--name jenkins jenkins/jenkins:lts
```

这里的三个挂载：

1. -v ~/jenkins:/var/jenkins_home ，将Jenkins数据本地持久化

2. -v /var/run/docker.sock:/var/run/docker.sock 和 -v /usr/bin/docker:/usr/bin/docker  目的是调用本地宿主机的docker命令

   ​

##### 相关问题

一、如果遇到执行Jenkins 使用docker报错的话："docker: error while loading shared libraries: libltdl.so.7: cannot open shared object file: No such file or directory"

则：在启动命令多家一个挂载的命令可以解决。

`-v /usr/lib64/libltdl.so.7:/usr/lib/x86_64-linux-gnu/libltdl.so.7`



二、（1）当Jenkins运行出现实例似乎已离线，执行以下操作

`1.vim /var/lib/jenkins/hudson.model.UpdateCenter.xml 	# 打开更新模块数据文件`

`2.https://updates.jenkins.io/update-center.json 改成 http://updates.jenkins.io/update-center.json`

`3.重启Jenkins服务`

 

（2）如果上述还是没有解决问题，执行以下操作

`1.vim vim /root/.jenkins/updates/default.json`

`2.将 "connectionCheckUrl":"http://www.google.com/" 改为 "connectionCheckUrl":"http://www.baidu.com/"`

`3.重启Jenkins`



备注：镜像加速：

`官方的镜像：http://mirrors.jenkins-ci.org/， http://archives.jenkins-ci.org/`

`清华大学镜像：https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json`

