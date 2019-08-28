FROM mongo:4.2


#COPY mongo.conf /usr/local/etc/mongo/mongo.conf
#COPY sources.list /etc/apt/sources.list

RUN apt-get update \
	&& apt-get install -yqq vim \
	&& apt-get install -yqq openssl \
	&& cp -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

CMD ["mongod"]

EXPOSE 27017