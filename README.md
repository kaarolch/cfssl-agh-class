# cfssl-agh-class
Cfssl CA example

1. Generate self-sign CA:
  ```
  mkdir temp
  cfssl gencert -initca -loglevel 0 ca-csr.json | cfssljson -bare temp/root-ca
  ```
2. Run CA server:
  ```
  cfssl serve -address=0.0.0.0 -port=8888 -config=ca-config.json -ca=temp/root-ca.pem -ca-key=temp/root-ca-key.pem
  ```
3. Request and sign client certificate (Use separate terminal):
  ```
  cfssl gencert -config=client-conf.json -remote=127.0.0.1 -profile=client client-req.json | cfssljson --bare temp/test123
  ```
4. Verify certificate and key md5 hash: 
  ```
  openssl rsa -modulus -noout -in temp/test123-key.pem | openssl md5
  openssl x509 -modulus -noout -in temp/test123.pem | openssl md5
  ```