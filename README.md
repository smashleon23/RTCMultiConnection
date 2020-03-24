# Setup steps to install WebRTC demo

## Install KAKFA server

Install kafka if not installed already (installed via docker)

git clone https://github.com/wurstmeister/kafka-docker.git 

```
cd kafka-docker
```

Install docker-compose (If you don't have kafka already and you want to use docker to install it)

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

change permissions
```
sudo chmod +x /usr/local/bin/docker-compose
```

Check version is installed
```
docker-compose --version
```

Using single node configuration

Do following changes to docker-compose-single-broker.yml

Remove
```
-      KAFKA_ADVERTISED_HOST_NAME: 192.168.99.100
-      KAFKA_CREATE_TOPICS: "test:1:1"

Add under environment
+      KAFKA_ADVERTISED_LISTENERS: INSIDE://kafka:9093,OUTSIDE://localhost:9092
+      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: INSIDE:PLAINTEXT,OUTSIDE:PLAINTEXT
+      KAFKA_LISTENERS: INSIDE://0.0.0.0:9093,OUTSIDE://0.0.0.0:9092
+      KAFKA_INTER_BROKER_LISTENER_NAME: INSIDE
```

Create images and boot containers
```
docker-compose -f docker-compose-single-broker.yml up
```



## Install web application

Install mvn (Disregard if already have node)

Install node version (if a node version is not already present)
```
nvm use 10.14.2
```

```
git clone git://github.com/smashleon23/RTCMultiConnection

cd RTCMultiConnection
```

Install dependencies
```
npm install
```

Create self signed certificates

```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /home/mlopez/Documentos/docs/KAMBDA/RTC_demo/self-signed.key -out /home/mlopez/Documentos/docs/KAMBDA/RTC_demo/self-signed.crt
```

Enable HTTPS and point to the SSL files, change config.json as so:
```
 "isUseHTTPs": "true",
  "sslKey": "/home/mlopez/Documentos/docs/KAMBDA/RTC_demo/self-signed.key",
  "sslCert": "/home/mlopez/Documentos/docs/KAMBDA/RTC_demo/self-signed.crt",
```

Run the server
```
node server --port=9001
```



