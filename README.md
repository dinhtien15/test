Hướng dẫn cài đặt
```
IP Planning
Hostname		      OS			               IP
AnsibleServer		  Centos7			          192.168.80.123
Client1			      Centos7			          192.168.80.124
Client2			      Centos7			          192.168.80.125
Client3			      Centos7			          192.168.80.126
```

B1. Cài đặt Ansible trên node Ansible Server
```
yum install -y epel-release 
yum update -y
yum install -y ansible
```
B2. Cấu hình SSH Key
Đứng tại user root của node AnsibleServer và thực hiện bước tạo key: 
```
ssh-keygen
```
Thực hiện các thao tác Enter và để mặc định các tùy chọn
```
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:ialESG10iMjC8ppgMWh46DaB34s7iuFwbUCB6a0FDP4 root@ansibleserver
The key's randomart image is:
+---[RSA 2048]----+
|X=o+...          |
|%@oo+.           |
|*=Oo.            |
|.B++.  o .       |
|+o=E..o S        |
|o..+..           |
|o ..+            |
|+oo.             |
|oo .             |
+----[SHA256]-----+
```
Thực hiện copy file key sang các node còn lại
```
ssh-copy-id root@192.168.80.124
ssh-copy-id root@192.168.80.125
ssh-copy-id root@192.168.80.126
```

B3. Tạo file inventory.ini
```
[kubernetes_master_nodes]
kubernetes-master ansible_host=192.168.100.102

[kubernetes_worker_nodes]
kubernetes-worker1 ansible_host=192.168.100.104
kubernetes-worker2 ansible_host=192.168.100.106

[kubernetes:children]
kubernetes_worker_nodes
kubernetes_master_nodes
```
B4. Gõ lệnh để kiểm tra trạng thái kết nối giữa server và client
```
Ansible all -m "ping"
```

Nếu kết nối ok thì chạy lệnh sau để chạy playbooks
```
Ansible-playbook -i inventory.ini k8scluster.yml
```
