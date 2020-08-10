# Usage

```
minikube start
```

## Make directories
```bash
minikube ssh
cd /mnt
sudo mkdir pv1 pv2 pv3
```


## Configure volumes to point to directories
```
kubectl apply -f pv/pv1.yml
kubectl apply -f pv/pv2.yml
kubectl apply -f pv/pv3.yml
kubectl get pv
```

## Claim volumes to pvc
```
kubectl apply -f pvc.yml
```

## Use pvc
```
kubectl apply -f mysql.yml
```

## Spin up mysql
```
kubectl apply -f mysql.yml
kubectl get pods -w
```

```bash
kubectl exec -ti mysql-xxx bash # mysql-xxx being your mysql pod name
mysql -ppassword
```

## Create stuff
```sql
create database test;
use test;
create table account (id INT);
insert into account values (1);
select * from account;
```

## Delete pod and see it being persistant
```bash
kubectl delete pod mysql-xxx
kubectl exec -ti mysql-yyy bash
mysql -ppassword
```
```sql
show databases;
use test;
show tables;
select * from account;
```

... Play around. Improvise.

## Troubleshoot not working
```bash
minikube ssh
sudo vi /etc/systemd/network/10-eth1.network # add DNS=8.8.8.8 under [Network]
sudo vi /etc/systemd/network/20-dhcp.network # add DNS=8.8.8.8 under [Network]
sudo systemctl restart systemd-networkd
# To test it execute something that has to resolve using dns, like curl google.com or docker pull
```