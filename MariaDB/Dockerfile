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
RUN \
  sed -i 's/^\(bind-address\s.*\)/# \1/' /etc/mysql/my.cnf && \
  echo "mysqld_safe &" > /tmp/config && \
  echo "mysqladmin --silent --wait=30 ping || exit 1" >> /tmp/config && \
  echo "mysql -e 'GRANT ALL PRIVILEGES ON *.* TO \"root\"@\"%\" WITH GRANT OPTION;'" >> /tmp/config && \
  bash /tmp/config && \
  rm -f /tmp/config

# Define mountable directories.
VOLUME ["/etc/mysql", "/var/lib/mysql"]

# Define working directory.
WORKDIR /data

#COPY ./schema.sql /root/
#COPY ./images.sql /root/
#COPY ./data.sql /root/


# Define default command.
CMD ["mysqld_safe"]

# Expose ports.
EXPOSE 3306

# Set TERM env to avoid mysql client error message "TERM environment variable not set" when running from inside the container
ENV TERM xterm