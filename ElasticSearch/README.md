# Dockerfile
## Elasticsearch latest:V2.xx

FROM ubuntu:14.04


## イメージのビルド
dockerfileを置くフォルダを作成  
`mkdir -p docker/es/`

フォルダにDockerfileなどをコピー  

ビルド実行  
`docker build -t maishiro/es docker/es/`

Volumeを作成  
`docker volume create --name esdata`


## コンテナの起動
`docker run -it -d --name myes -p 9200:9200 -v esdata:/usr/share/elasticsearch/data maishiro/es`


## machine ipの確認
`docker-machine ip`
> 192.168.99.101

## Elasticsearchへのアクセス
http://192.168.99.101:9200/

## elasticsearch-headへのアクセス
http://192.168.99.101:9200/_plugin/head/

## Elastic HQへのアクセス
http://192.168.99.101:9200/_plugin/hq/



## Volumeデータの取り出し
`docker run --rm --volumes-from myes -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /usr/share/elasticsearch/data`




### 環境
* Windows 10
* Docker Toolbox
* VirtualBox


### 参考
How to make a Dockerfile for Elasticsearch  
https://www.elastic.co/blog/how-to-make-a-dockerfile-for-elasticsearch

elasticsearch-head  
https://github.com/mobz/elasticsearch-head

Elastic HQ  
http://www.elastichq.org/

Backup, restore, or migrate data volumes  
https://docs.docker.com/engine/tutorials/dockervolumes/


