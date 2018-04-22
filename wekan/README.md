### Sorry, under construction...
- - -

#### create wekan user

``` shell-session
useradd \
> -d /home/wekan
> -m
> -s /bin/bash
> wekan
usermod -aG docker wekan
passwd wekan
su - wekan
```

####  Nginx proxy

``` shell-session
mkdir -p /var/opt/docker/wekan/wekanproxy/{conf.d,ssl}
```


#### start containers

``` shell-session
mkdir ~wekan/docker
touch ~wekan/docker/docker-compose.yml
docker-compose up -d
docker-compose logs
```
