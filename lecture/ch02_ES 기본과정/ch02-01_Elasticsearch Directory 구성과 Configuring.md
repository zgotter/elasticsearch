# 1. Elasticsearch Directory 구성과 Configuring

## 1.0 ES와 Kibana 환경 구성

### 1.0.1 Single node ES 구성 및 실행

```
cd ./_downloads

tar -xvzf elasticsearch-7.15.0-darwin-x86_64.tar.gz
mv elasticsearch-7.15.0 ../workspace/ch02
```

```
cd ./workspace/ch02

ln -s elasticsearch-7.15.0 elasticsearch

cd elasticsearch
vi config/elasticsearch.yml
```

```
cluster.name: training-cluster
node.name: training-node
discovery.type: single-node
```

```
bin/elasticsearch
```

<br>

### 1.0.2 Kibana 구성 및 실행

```
cd ./_downloads

tar -xvzf kibana-7.15.0-darwin-x86_64.tar.gz
mv kibana-7.15.0-darwin-x86_64 ../workspace/ch02
```

```
cd ./workspace/ch02

ln -s kibana-7.15.0-darwin-x86_64 kibana
```

```
bin/kibana
```

<br>

## 1.1 Directory 구성

```
|-- LICENSE.txt
|-- NOTICE.txt
|-- README.asciidoc
|-- bin
|-- config
|-- data
|-- jdk.app
|-- lib
|-- logs
|-- modules
|-- plugins
```

- `bin`: 실행 스크립트
- `config`: 설정 파일
- `data`: index
  - es를 실행시키는 시점에 해당 경로 생성됨
- `idk.app`: jdk
- `lib`: elasticsearch와 dependency jar
- `logs`: elasticsearch 실행 로그
- `modules`: elasticsearch에서 사용하는 모듈 jar
- `plugins`: elasticsearch에서 사용하는 플러그인 jar
  - arirang, nori 같은 것들이 해당 경로 하위에 위치

<br>

- `data`와 `logs` 는 `path.data`, `path.logs` 설정으로 변경 가능
- `config` 는 classpath 기본 위치로 사용
  - 형태소 분석기 설치 시 사전 파일, 동의어 파일의 classpath 위치가 `config`로 잡히기 때문에 해당 경로 하위에 생성한다.

<br>

## 1.2 Configuring

- ES를 구성하기 위한 기본 설정은 `config/elasticsearch.yml` 을 통해 할 수 있다.
- 단일 노드로 구성할 경우 기본값이 어떻게 되는 지 정도는 알고 있으면 유리
- `cluster.name` 과 `node.name` 은 클러스터와 노드를 잘 운영하기 위해 반드시 설정
  - 설정하지 않을 경우 기본값으로 지정됨
    - `cluster.name: elasticsearch`
    - `node.name: hostname`
