# Crypto

### Create self-signed x509 cert

```bash
CN=${1:-cn.invalid.com}

SUBJECT_KEY=$(mktemp)
CSR=$(mktemp)

openssl req -new \
-newkey rsa:2048 -nodes -keyout ${SUBJECT_KEY} \
-out ${CSR} \
-subj "/C=FI/ST=Uusimaa/L=Helsinki/O=Company/OU=Department/CN=${CN}"

CRT=$(mktemp)
CA=$(mktemp)
ISSUER_KEY=$(mktemp)
openssl req -x509 -newkey rsa:2048 -keyout ${ISSUER_KEY} -out ${CA} -days 365 \
-subj "/C=FI/ST=Uusimaa/L=Helsinki/O=Company/OU=Department/CN=${CN}"

openssl x509 -req -in ${CSR} -CA ${CA} -CAkey ${ISSUER_KEY} -CAcreateserial -days 730 -sha256
```
