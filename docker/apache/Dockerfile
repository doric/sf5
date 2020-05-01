FROM ubuntu:latest
MAINTAINER Ngouffo Doric <ngouffodoric@gmail.com>

# Locale
ENV LANGUAGE fr_FR.UTF-8
ENV LOCALE fr_FR.UTF-8
ENV LANG fr_FR.UTF-8
ENV LC_ALL=en_US.UTF-8

# PHP Timezone
ENV TZ=Europe/Paris


#epel release and php extension for symfony 5
RUN apt update -y && \
    apt upgrade -y && \
    apt-get install -y language-pack-en-base && \
    apt install -y software-properties-common  && \
    add-apt-repository ppa:ondrej/php && \
    apt update -y && \
    apt install -y apache2 libapache2-mod-php vim sudo php7.3 php7.3-cli php7.3-common php7.3-fpm php7.3-mysql php-apcu php7.3-curl php7.3-intl php7.3-mbstring php7.3-xml && \
    apt clean all

COPY conf/sf5.conf /etc/apache2/sites-available/
#COPY script/*      /usr/local/apache2/bin/

RUN apt-get install -y debhelper lsb-release libaprutil1-dev libapr1-dev libpcre3-dev zlib1g-dev libnghttp2-dev libssl-dev perl liblua5.2-dev libxml2-dev autotools-dev gawk dh-systemd
RUN export PATH="/usr/local/apache2/bin:$PATH"

#RUN find /etc/apache2/sites-available/ -type f -exec basename '{}' \; | xargs -n 1 a2dissite && \
RUN a2dissite 000-default && \
    a2ensite  sf5 &&\
    whereis apachectl &&\
    a2enmod rewrite &&\
    a2enmod negotiation

#Requis par MariaDB pour pouvoir utiliser le client MariaDB depuis l'intérieur du container
RUN echo "\nexport TERM=xterm" >> ~/.bashrc && \
    mkdir -p "/var/run/apache2/" "/var/run/apache2$SUFFIX" "/var/lock/apache2$SUFFIX"

# Starting up Apache
CMD ["/usr/sbin/apache2", "-DFOREGROUND"]

# Exposition du port 80 servi par apache
EXPOSE 80

#Définitions d'un volume pour les fichiers de MariaDB
VOLUME ["/root/.cache/", "/var/www/sf5"]

