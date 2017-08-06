# openssl-tutorial

## 1.- Create de CA private and public keys:

	$ openssl genrsa -aes256 -out ca-key.pem 2048
	$ openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem

## 2.- Create the server key and the certificate signing request

	$ openssl genrsa -out server-key.pem 1024
	$ openssl req -subj "/CN=server.domain.com" -new -key server-key.pem -out server.csr

## 3.- To allow connections from localhost and a specific IP:

	$ echo "subjectAltName = IP:127.0.0.1,IP:192.168.4.23" > extension.cnf
	$ openssl x509 -req -days 365 -in server.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out server-cert.pem -extfile extension.cnf

## 4.- Client authentication: client key and certificate signing request

	$ openssl genrsa -out key.pem 20148
	$ openssl req -subj '/CN=client.domain.com' -new -key key.pem -out client.csr

## 5.- Key suitable for client authentication

	$ echo "extendedKeyUsage = clientAuth" > client_extension.cnf
	$ openssl x509 -req -days 365 -in client.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out cert.pem -extfile client_extension.cnf

## 6.- Results:

	CA:     ca-key.pem     ca.pem
	server: server-key.pem server-cert.pem
	client: key.pem        cert.pem

## PEM to PKCS

	$ openssl pkcs12 -export -clcerts -in server-cert.pem -inkey server-key.pem -out server.p12

## PKCS to PEM

	$ openssl pkcs12 -clcerts -in server.p12 -out server.pem

