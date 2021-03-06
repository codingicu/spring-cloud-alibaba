# docker-compose说明

```yaml
version: "3.9"

services:

  mysql:
    image: mysql
    ports:
      - 3306:3306
    environment:
      MYSQL_ROOT_PASSWORD: root

  nacos:
    image: nacos/nacos-server
    environment:
      - MODE=standalone
      - SPRING_DATASOURCE_PLATFORM=mysql
      - MYSQL_SERVICE_HOST=192.168.0.100
      - MYSQL_SERVICE_PORT=3306
      - MYSQL_SERVICE_DB_NAME=nacos
      - MYSQL_SERVICE_USER=root
      - MYSQL_SERVICE_PASSWORD=root
    ports:
      - 8848:8848

  sentinel:
    build: sentinel
    ports:
      - 18080:8080

  rokcetmq:
    build: rocketmq
    ports:
      - 9876:9876

  seata:
    image: seataio/seata-server
    hostname: seata-server
    ports:
      - 8091:8091
    environment:
      - SEATA_IP=192.168.0.102
      - SEATA_PORT=8091
      - STORE_MODE=db
      - SEATA_CONFIG_NAME=file:/root/seata-config/registry
    volumes:
      - /seata/conf:/root/seata-config

  zookeeper:
    image: zookeeper
    ports:
      - 2181:2181

  dubbo-admin:
    image: apache/dubbo-admin
    ports:
      - 28080:8080
    depends_on:
      - zookeeper
    environment:
      - admin.registry.address=zookeeper://zookeeper:2181
      - admin.config-center=zookeeper://zookeeper:2181
      - admin.metadata-report.address=zookeeper://zookeeper:2181
```
