openssl genrsa -des3 -out server.key 2048
openssl req -new -x509 -key server.key -out server.crt -days 18250
openssl pkcs12 -export -out server.pfx -inkey server.key -in server.crt

pass exportpassword 147258369
