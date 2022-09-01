# 4. Kibana

## 4.1 Kibana란

- 저장된 데이터를 조회하고 분석할 수 있도록 기능을 제공해주는 도구
- Elastic 사에서는 Kibana를 Elasticsearch의 공식 Interface라고 이야기한다.
- 초기
  - Dashboard, Visualization을 제공하기 위한 용도
- 현재
  - Elastic Stack을 운영하고 모니터링하는 용도까지 확대

- Kibana는 단독으로 사용할 수 없고 ES와 함께 사용해야 한다.

<br>

## 4.2 Kibana 실행 w/ tar ball

```
cd elasticsearch-7.15.0
bin/elasticsearch

cd kibana-7.15.0-darwin-x86_64
bin/kibana
```

- 접속
  - http://localhost:5601

<br>

## 4.3 start.sh & stop.sh

- Kibana는 ES처럼 백그라운드로 실행시키는 옵션이 존재하지 않는다.
- 그래서 Kibana를 백그라운드에서 실행시키고 종료시킬 수 있는 shell script를 정의한다.

<br>

### 4.3.1 start.sh

```she
#!/bin/bash

DIR_NAME=`dirname "$0"`
DIR_HOME=`cd $DIR_NAME; pwd`

$DIR_HOME/bin/kibana> /dev/null 2>&1 & KIBANA_PID=$!
echo $KIBANA_PID > $DIR_HOME/bin/kibana.pid
```

<br>

### 4.3.2 stop.sh

```she
#!/bin/bash

DIR_NAME=`dirname "$0"`
DIR_HOME=`cd $DIR_NAME; pwd`

KIBANA_PID=`cat $DIR_HOME/bin/kibana.pid`
kill -9 $KIBANA_PID

echo "Kibana Daemon($KIBANA_PID) is killed"
rm -f $DIR_HOME/bin/kibana.pid
```

<br>

## 4.4 Kibana 실행 w/ docker-compose

### 4.4.1 기본

```
# docker-compose-kibana.yml

version: "3.7"
services:
  docker-kibana:
    image: docker.elastic.co/kibana/kibana:7.15.0
    container_name: docker-kibana
    environment:
      ELASTICSEARCH_HOSTS: '["http://host.docker.internal:9200"]'
    ports:
      - 5601:5601
    expose:
      - 5601
#    volumes:
#      - ./kibana-config.yml:/usr/share/kibana/config/kibana.yml
    restart: always
    network_mode: bridge
```

```
cd ./workspace/docker
docker-compose -f docker-compose-kibana.yml up -d
```

```
docker-compose -f docker-compose-kibana.yml down
```

<br>

### 4.4.2 Kibana 설정 파일 volumes 마운트

- kibana의 docker-compose.yml 파일에 ES 접속 정보를 volume을 통해 외부 yml 파일을 이용하여 전달할 수도 있다.

```
# docker-compose-kibana.yml

version: "3.7"
services:
  docker-kibana:
    image: docker.elastic.co/kibana/kibana:7.15.0
    container_name: docker-kibana
#    environment:
#      ELASTICSEARCH_HOSTS: '["http://host.docker.internal:9200"]'
    ports:
      - 5601:5601
    expose:
      - 5601
    volumes:
      - ./kibana-config.yml:/usr/share/kibana/config/kibana.yml
    restart: always
    network_mode: bridge
```

```
# kibana-config.yml

server.host: "0.0.0.0"
elasticsearch.hosts: ["http://host.docker.internal:9200"]
```

