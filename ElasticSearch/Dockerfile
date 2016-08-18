FROM ubuntu:14.04

ENV DEBIAN_FRONTEND=noninteractive

RUN sudo apt-get update && \
    sudo apt-get install -y --no-install-recommends software-properties-common && sudo add-apt-repository -y ppa:webupd8team/java && \
    sudo apt-get update && \
    (echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections) && \
    sudo apt-get install --no-install-recommends -y oracle-java8-installer && \
    rm -rf /var/cache/oracle-jdk8-installer && \
    echo "networkaddress.cache.ttl=60" >> /usr/lib/jvm/java-8-oracle/jre/lib/security/java.security && \
    sudo apt-get clean && rm -rf /var/lib/apt/lists/*
ENV JAVA_HOME /usr/lib/jvm/java-8-oracle

RUN groupadd -g 1000 elasticsearch && useradd elasticsearch -u 1000 -g 1000

#RUN sudo apt-key adv --keyserver pgp.mit.edu --recv-keys 46095ACC8548582C1A2699A9D27D666CD88E42B4 && \
#    sudo add-apt-repository -y "deb http://packages.elastic.co/elasticsearch/2.x/debian stable main" --keyserver https://pgp.mit.edu/ && \
#    sudo apt-get update && \
#    sudo apt-get install -y --no-install-recommends elasticsearch
RUN wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add - && \
    echo "deb http://packages.elastic.co/elasticsearch/2.x/debian stable main" | sudo tee -a /etc/apt/sources.list.d/elasticsearch-2.x.list && \
    sudo apt-get update && \
    sudo apt-get install -y --no-install-recommends elasticsearch

WORKDIR /usr/share/elasticsearch

RUN set -ex && for path in data logs config config/scripts; do \
        mkdir -p "$path"; \
        chown -R elasticsearch:elasticsearch "$path"; \
    done

COPY logging.yml /usr/share/elasticsearch/config/
COPY elasticsearch.yml /usr/share/elasticsearch/config/

USER elasticsearch

ENV PATH=$PATH:/usr/share/elasticsearch/bin

# 
RUN /usr/share/elasticsearch/bin/plugin install mobz/elasticsearch-head
RUN /usr/share/elasticsearch/bin/plugin install royrusso/elasticsearch-HQ
# 
RUN /usr/share/elasticsearch/bin/plugin install analysis-kuromoji
# 
RUN /usr/share/elasticsearch/bin/plugin install lang-python

CMD ["elasticsearch"]

EXPOSE 9200 9300
