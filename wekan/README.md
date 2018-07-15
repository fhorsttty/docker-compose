# Sorry, under construction...

> Note:
> We suggests that the docker engine activates `user namespaces` function.


## Create wekan user

``` shell-session
useradd \
> -d /home/wekan \
> -m \
> -s /bin/bash \
> wekan
usermod -aG docker wekan    # wekanユーザーをdockerグループに追加する。
passwd wekan
su - wekan
```


##  Set up nginx proxy


Edit `docker-compose.yml` according to your environment. 

``` shell-session
git clone https://github.com/fhorsttty/docker-compose-examples.git
cp -R docker-compose-examples/wekan .
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
