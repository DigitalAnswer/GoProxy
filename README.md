I wrote this code to apply the following tutorial: [medium](https://medium.com/@mlowicki/http-s-proxy-in-golang-in-less-than-100-lines-of-code-6a51c2f2c38c)

Except: I didn't generate ssl certificate.

but if you want to generate it you can find this script:
```shell
#!/usr/bin/env bash
case `uname -s` in
    Linux*)     sslConfig=/etc/ssl/openssl.cnf;;
    Darwin*)    sslConfig=/System/Library/OpenSSL/openssl.cnf;;
esac
openssl req \
    -newkey rsa:2048 \
    -x509 \
    -nodes \
    -keyout server.key \
    -new \
    -out server.pem \
    -subj /CN=localhost \
    -reqexts SAN \
    -extensions SAN \
    -config <(cat $sslConfig \
        <(printf '[SAN]\nsubjectAltName=DNS:localhost')) \
    -sha256 \
    -days 3650
```

sample curl request:
```shell
curl -Lv --proxy-insecure --proxy https://localhost:9090 http://google.com
```