# Création d'un certificats TLS pour un service


Pour montrer cette explication nous allons utiliserons l'arboresence suivante afin de chiffrer les communication vers un sute web

```bash
├── autorite
│├── certificats
│└── keys
└── siteweb
├── certificats
├── keys
└── requests_certificats
```



### On genere la clé privée du site web
```bash 
openssl genrsa 2048 > siteweb/keys/privatekey.key
```

### On genere le certificats du site web

```bash
openssl req -new -key siteweb/keys/privatekey.key > siteweb/requests_certificats/demande.csr
```

### On signe le certificat via l'authorité

```bash
openssl x509 -req -in siteweb/requests_certificats/demande.csr   -out siteweb/certificats/certificat.crt -CA autorite/certificats/ca.crt -CAkey autorite/key/private_ca.key -CAcreateserial -CAserial autorite/certificats/ca.srl
```

### On peut vérifier 

openssl req -in siteweb/requests_certificats/demande.csr -noout -text
 