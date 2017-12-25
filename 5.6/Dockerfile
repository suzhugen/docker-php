FROM centos:7
USER root

RUN groupadd www && useradd -g www -s /sbin/nologin -M www  

RUN yum install -y libxml2-devel  gcc glibc  openssl openssl-devel  bzip2 bzip2-devel libzip  curl-devel   libXpm-devel  readline-devel   libjpeg-devel  libpng-devel  wget  tar  gmp-devel \
&& wget ftp://mcrypt.hellug.gr/pub/crypto/mcrypt/attic/libmcrypt/libmcrypt-2.5.7.tar.gz && tar -zxf libmcrypt-2.5.7.tar.gz && cd libmcrypt-2.5.7 && ./configure prefix=/usr/local && make && make install

RUN wget http://cn2.php.net/distributions/php-5.6.32.tar.gz \
&& tar zxf php-5.6.32.tar.gz \
&& cd php-5.6.32 && chmod -R 777 configure \
&& ./configure --prefix=/usr/local/php  --enable-fpm  --with-fpm-user=www --with-fpm-group=www \
--with-mcrypt --enable-pdo \
--enable-mbstring  --with-curl --disable-debug  --disable-rpath \
--enable-inline-optimization --with-bz2  --with-zlib --enable-sockets \
--enable-sysvsem --enable-sysvshm --enable-pcntl --enable-mbregex --enable-opcache=no \
--with-mhash --enable-zip --with-pcre-regex \
--with-mysql --with-mysqli  --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd \ 
--with-gd --with-jpeg-dir \
--with-gettext --enable-mbstring --with-iconv --with-mcrypt --with-mhash --with-openssl --enable-bcmath --enable-soap --with-libxml-dir --enable-pcntl --enable-shmop --enable-sysvmsg --enable-sysvsem --enable-sysvshm --enable-sockets --with-curl --with-zlib --enable-zip --with-bz2 --with-readline --without-sqlite3 --without-pdo-sqlite --with-pear \
&& make all install

RUN rm -f ../php-5.6.32.tar.gz
RUN { echo 'PATH=$PATH:/usr/local/php/bin'; \
      echo 'export PATH';\
    } | tree -a  /etc/profile && source /etc/profile


RUN mkdir -p /data/www && chmod -R 655 /data/www
VOLUME [ "/data/www" ]


COPY entrypoint-my /usr/local/bin/
RUN chmod -R 777 /usr/local/bin/entrypoint-my

EXPOSE 9000 

CMD [ "entrypoint-my" ]