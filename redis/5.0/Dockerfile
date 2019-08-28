FROM redis:5.0

COPY sources.list /etc/apt/sources.list

RUN apt-get update \
	&& apt-get install -yqq vim \
	&& apt-get install -yqq openssl \
	&& cp -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
#RUN mkdir -p /usr/local/etc/redis
#COPY redis.conf /usr/local/etc/redis/redis.conf

VOLUME /data

EXPOSE 6379

CMD ["redis-server", "/usr/local/etc/redis/redis.conf"]


