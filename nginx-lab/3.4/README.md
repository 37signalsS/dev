## Mutual TLS (mTLS) Setup

curl --resolve site1.local:443:192.168.77.11 \
               --insecure \
               --key ca/my-service.key \
               --cert ca/my-service.crt \
               https://site1.local/private_api
