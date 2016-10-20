#
# MariaDB Dockerfile
#
#

# Pull base image.
FROM resin/rpi-raspbian:jessie

ENV \
  DB_max_allowed_packet=64M \
  DB_query_cache_size=0 \
  DB_query_cache_type=0 \
  DB_sync_binlog=0 \
  DB_innodb_buffer_pool_size=768M \
  DB_innodb_log_file_size=128M \
  DB_innodb_flush_method=O_DIRECT \
  DB_innodb_old_blocks_time=1000 \
  DB_innodb_flush_log_at_trx_commit=0 \
  DB_open_files_limit=4096 \
  DB_max_connections=300


# Install MariaDB.
RUN \
  sudo apt-get update && \
  sudo apt-get -y install wget && \
  sudo wget -O /etc/apt/sources.list.d/repository.pi3g.com.list http://repository.pi3g.com/sources.list && \
  wget -O - http://repository.pi3g.com/pubkey | sudo apt-key add - && \
  sudo apt-get update && \                                                         
  sudo apt-get upgrade  && \
  apt-get -y install mariadb-server

# clean all package
RUN \
  sudo apt-get clean  

# Define working directory.
WORKDIR /data


# MySQL Config & Zabbix Schema
ADD ./mysql/my.cnf /etc/mysql/conf.d/my.cnf
ADD ./create/data.sql /tmp/create/data.sql
ADD ./create/images.sql /tmp/create/images.sql
ADD ./create/schema.sql /tmp/create/schema.sql


# Custom script to setup Zabbix schema and start mysqld_safe
ADD ./scripts/start.sh /start.sh
RUN chmod 755 /start.sh

# Expose ports.
EXPOSE 3306

# Define mountable directories.
VOLUME ["/etc/mysql", "/var/lib/mysql"]

# Set TERM env to avoid mysql client error message "TERM environment variable not set" when running from inside the container
ENV TERM xterm

# Sart the custom start script
CMD ["/bin/bash", "/start.sh"]