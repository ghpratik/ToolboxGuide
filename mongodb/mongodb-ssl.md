- Generate self asigned certificate and key for testing purpose

```openssl req -newkey rsa:2048 -new -x509 -days 365 -nodes -out mongodb-cert.crt -keyout mongodb-cert.key```

