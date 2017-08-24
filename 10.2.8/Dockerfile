FROM centos:7

ENV MARIADB_VER 10.2.8

RUN set -xe && \
    yum install -y  \
        bash \
        make \
        mariadb=${MARIADB_VER} \
        tzdata \
        go \
        git \
        wget

RUN  export GOPATH=$HOME/go && \
    go get github.com/tsg/gotpl

RUN groupadd mysql
RUN useradd -s /sbin/nologin -g mysql mysql
RUN mkdir -p /var/run/mysqld && mkdir -p /var/lib/mysql
RUN chown -R mysql:mysql /var/run/mysqld && chown -R mysql:mysql /var/lib/mysql

RUN mkdir /docker-entrypoint-initdb.d
COPY actions /usr/local/bin

RUN chown mysql:mysql /usr/local/bin/actions.mk /usr/local/bin/wait-for-mariadb.sh

#COPY *.tpl /etc/gotpl/
COPY docker-entrypoint.sh /

WORKDIR /var/lib/mysql
VOLUME /var/lib/mysql

EXPOSE 3306

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["mysqld"]
