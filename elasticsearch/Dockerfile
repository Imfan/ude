FROM docker.elastic.co/elasticsearch/elasticsearch:7.3.0
## development mode
ENV discovery.type=single-node


#COPY --chown=elasticsearch:elasticsearch elasticsearch.yml /usr/share/elasticsearch/config/

# 9200作为Http协议，主要用于外部通讯，ES集群之间是通过9300进行通讯
EXPOSE 9200 9300
