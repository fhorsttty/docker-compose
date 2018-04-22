This directory needs to 3 files.

- private.key
- self_signed.crt
- dh__param.pem


Generate private-key and Self-Signed certification.
``` shell-session
openssl req -x509 -nodes -day 3650 -newkey rsa:2048 \
> -keyout private.key \
> -out self_signed.crt
```


Generate dhparam file.
```
openssl dhparam -out dh_param.pem 4096
```
