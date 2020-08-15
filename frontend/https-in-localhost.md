## Generate certificate 

Open **CMD** and type: `openssl req -x509 -newkey rsa:2048 -keyout keytmp.pem -out cert.pem -days 365` then
`openssl rsa -in keytmp.pem -out key.pem`


## Create a `.env` file:

```
HTTPS=true

PORT=8080

HOST=localhost

SSL_CRT_FILE=cert.pem

SSL_KEY_FILE=key.pem
```

That's it! :kissing_heart:
