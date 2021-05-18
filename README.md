# Linux HW 5

Vagantfile содержит описание 2х ВМ:  
```
nfs-server 192.168.111.110
nfs-client 192.168.111.115
```
Эти ВМ так же описаны в inventory-файле `inventories/nfs/all.yml`  

Плэйбук `nfs.yml` вызывает 3 роли из диретокрии `roles`  
Монтировование nfs-шары сделано через два systemd юнита: mnt-share-upload.automount и mnt-share-upload.mount  

Проект использует pipenv для установки ansible  
Как установить проект:  

```
cd otus-linux-hw5
pip install pipenv
pipenv shell
pipenv install
```

Как запустить проект:
```
vagrant up
ansible-playbook nfs.ym -v
```
На сервере:
```
rpcinfo -p

100005    1   udp  20048  mountd
100005    1   tcp  20048  mountd
100005    2   udp  20048  mountd
100005    2   tcp  20048  mountd
100005    3   udp  20048  mountd
100005    3   tcp  20048  mountd
100003    3   tcp   2049  nfs
100003    4   tcp   2049  nfs
100227    3   tcp   2049  nfs_acl
100003    3   udp   2049  nfs
100003    4   udp   2049  nfs
100227    3   udp   2049  nfs_acl
```

На клиенте:
```
[vagrant@nfs-client ~]$ echo test1 > /mnt/share/upload/test1
[vagrant@nfs-client ~]$ ls -alh /mnt/share/upload/
total 4.0K
drwxrwxrwx. 2 root    root    19 May 18 23:12 .
drwxr-xr-x. 3 root    root    20 May 18 23:10 ..
-rw-rw-r--. 1 vagrant vagrant  6 May 18 23:25 test1
```
