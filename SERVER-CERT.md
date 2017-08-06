# openssl-tutorial

## 1.- Create de CA private and public keys:

	$ openssl genrsa -aes256 -out ca-key.pem 2048
	$ openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem

## 2.- Create the server key and the certificate signing request

	$ openssl genrsa -out server-key.pem 1024
	$ openssl req -subj "/CN=server.domain.com" -new -key server-key.pem -out server.csr

## 3.-

