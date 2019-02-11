> Note:
> We suggests that the docker engine activates `user namespaces` function.


## Create wekan user

``` shell-session
useradd \
> -d /home/wekan \
> -m \
> -s /bin/bash \
> wekan
chmod 755 /home/wekan
usermod -aG docker wekan    # wekanユーザーをdockerグループに追加する。
passwd wekan
su - wekan
```


##  Set up nginx proxy


Edit `docker-compose.yml` according to your environment. 

``` shell-session
git clone https://github.com/fhorsttty/docker-compose-examples.git
cp -R docker-compose-examples/wekan/* .
```


Change directory to `wekanproxy/ssl/`.

```shell-session
cd wekanproxy/ssl/
```


This `ssl` directory needs 3 files.

- private.key
- self_signed.crt
- dh_param.pem


### Generate private-key and self-signed certification.

``` shell-session
openssl req -x509 -nodes -days 3650 -newkey rsa:2048 \
> -keyout private.key \
> -out self_signed.crt
chmod 644 private.key
```


### Generate dhparam file.


This takes a long times. Please be patient.
```
openssl dhparam -out dh_param.pem 4096
```


## Start containers


``` shell-session
cd ~wekan
docker-compose up -d
docker-compose logs
```


## Backup and upgrade Wekan


Stop all containers and start the container of wekandb.

``` shell-session
# host-os
cd ~wekan
docker-compose stop
docker-compose up -d wekandb
docker exec -it wekan-db bash
```


Dump the backup data of mongodb.

``` shell-session
# container
cd /
rm -rf /dump/*
mongodump -o /dump
ls -la /dump
exit
```


Copy the backup data to local directory.

``` shell-session
docker cp wekan-db:/dump wekandb/
ls -la wekandb/dump/wekan
docker-compose stop wekandb
```


Edit the version of wekan in `docker-compose.yml`.
Upgrade and start containers.

``` shell-session
docker-compose up -d
```


## Restore Wekan

Remove the container and the volume of mongodb.

``` shell-session
# host-os
cd ~wekan
docker-compose stop wekandb
docker ps -a | grep wekan-db
docker rm <wekan-db-container-id>
docker volume ls
docker volume rm wekan_wekan-db
```


``` shell-session
# host-os
docker-compose up -d wekandb
docker-compose ps
docker cp wekandb/dump wekan-db:/data
docker exec -it wekan-db bash
```


``` shell-session
# container
cd /data
mongorestore --drop --db wekan dump/wekan
rm -rf dump/*
exit
```


``` shell-session
# host-os
docker-compose stop wekandb
docker-compose ps
docker-compose up -d
rm -rf wekandb/dump/*
```
