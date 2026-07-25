## Jenkins 
- CICD
![CICD](./images/cicd.png)


## change jenkins port 
**update this two files in jenkins server**
- root@ip-172-31-44-239:~# `vim /etc/default/jenkins`
- root@ip-172-31-44-239:~# `vim /usr/lib/systemd/system/jenkins.service`
```
systemctl restart jenkins
```

