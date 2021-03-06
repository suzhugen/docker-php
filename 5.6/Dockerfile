FROM centos:7
USER root

RUN set -xe; \
if ! id www ;then \
    echo "create user "; \
    groupadd www; \
    useradd -g www -s /sbin/nologin -M www;  \
fi;

RUN yum install -y libxml2-devel  gcc glibc  openssl openssl-devel  bzip2 bzip2-devel libzip  curl-devel \
  libXpm-devel  readline-devel    libjpeg-devel  libpng-devel  wget  tar  gmp-devel \
&& wget ftp://mcrypt.hellug.gr/pub/crypto/mcrypt/attic/libmcrypt/libmcrypt-2.5.7.tar.gz \
 && tar -zxf libmcrypt-2.5.7.tar.gz && cd libmcrypt-2.5.7 && ./configure prefix=/usr/local \
 && make && make install

RUN wget http://mirror.csclub.uwaterloo.ca/nongnu/freetype/freetype-2.4.0.tar.bz2 \ 
&& tar -jxf freetype-2.4.0.tar.bz2 && cd freetype-2.4.0 \
&& ./configure --prefix=/usr/local/freetype && make && make install

RUN { echo "/usr/local/lib64 ";\
      echo "/usr/local/lib ";\
     echo " /usr/lib";\
     echo "/usr/lib64"; } | tee -a /etc/ld.so.conf && ldconfig -v

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
--with-gettext --with-iconv --with-openssl \ 
--enable-bcmath --enable-soap --with-libxml-dir --enable-shmop --enable-sysvmsg \
  --with-readline --without-sqlite3 --without-pdo-sqlite --with-pear \
--with-freetype-dir=/usr/local/freetype \
&& make all install


RUN { echo 'PATH=$PATH:/usr/local/php/bin'; \
      echo 'export PATH';\
    } | tee -a  /etc/profile && source /etc/profile 

RUN mkdir -p /usr/local/php/etc/fpm.d/

COPY php-fpm.conf /usr/local/php/etc/php-fpm.conf
COPY zz-docker.conf /usr/local/php/etc/fpm.d/zz-docker.conf

# RUN cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf  
RUN  cp /usr/local/php/sbin/php-fpm /usr/local/bin/php-fpm && chmod +x /usr/local/bin/php-fpm \
&&  cp /usr/local/php/bin/php /usr/local/bin/php && chmod +x /usr/local/bin/php


RUN mkdir -p /data/www && chmod -R 655 /data/www
VOLUME [ "/data/www" ]


COPY entrypoint-my /usr/local/bin/
RUN chmod -R 777 /usr/local/bin/entrypoint-my

# clear rubbish
RUN rm -f ../php-5.6.32.tar.gz
RUN rm -rf /php-5.6.32/

EXPOSE 9000 

CMD [ "entrypoint-my" ]
