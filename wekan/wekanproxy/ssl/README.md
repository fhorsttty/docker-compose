### This directory needs 3 files.

- private.key
- self_signed.crt
- dh_param.pem


#### Generate private-key and Self-Signed certification.

``` shell-session
openssl req -x509 -nodes -days 3650 -newkey rsa:2048 \
> -keyout private.key \
> -out self_signed.crt
```


#### Generate dhparam file.

This takes a long times. Please be patient.
```
openssl dhparam -out dh_param.pem 4096
```
