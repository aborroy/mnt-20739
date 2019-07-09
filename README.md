# Alfresco 5.2.3 with Sharding (DB_ID_RANGE)

This Docker Compose template provides a testing environment for SOLR Sharding using DB_ID_RANGE method for Search Services 1.3.0.3 and Alfresco Repository 5.2.3

## Docker Images

* Alfresco and Share built from https://releases.alfresco.com/Enterprise-5.2/5.2.3/build-00012/ALL/alfresco-content-services-distribution-5.2.3.zip (Alfresco VPN is required to access this URL)
* SOLR: alfresco/search-services:1.3.0.3 (from DockerHub)
* PostgreSQL: postgres 10.1 (from DockerHub)
* Libreoffice built from LibreOfice 5.3

## Usage

Starting the environment

```
$ docker-compose up --build
```

## Services

```
http://localhost:8080/share
http://localhost:8081/alfresco
http://localhost:8083/solr (Shard 1 with DB_ID range 0-200)
http://localhost:8084/solr (Shard 2 with DB_ID range 201-40000)
```

Default credentials

```
admin / admin
```
