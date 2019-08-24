FROM mysql:5.6

COPY sources.list /etc/apt/sources.list
COPY my.cnf /etc/mysql/my.cnf

RUN apt-get update \
	&& apt-get install -yqq vim \
	&& apt-get install -yqq logrotate \
	&& apt-get install -yqq openssl



#####################################
# Set Timezone
#####################################

ARG TZ=UTC
#RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone && chown -R mysql:root /var/lib/mysql/

ARG MYSQL_PASSWORD=${MYSQL_PASSWORD}
ENV MYSQL_ROOT_PASSWORD=$MYSQL_PASSWORD

CMD ["mysqld"]

EXPOSE 3306
