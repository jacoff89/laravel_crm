FROM debian:bookworm

RUN apt-get update && apt-get install -y \
    ca-certificates mailutils \
    libsasl2-2 libsasl2-modules rsyslog \
    libsasl2-modules-gssapi-mit sasl2-bin \
    tzdata postfix opendkim opendkim-tools supervisor \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN apt-get update
RUN apt-get install -y mc iproute2 dnsutils inetutils-telnet iputils-ping nano \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /var/spool/postfix-opendkim/opendkim \
    /etc/opendkim/keys /var/log/supervisor

COPY ./config/postfix/main.cf /etc/postfix/main.cf
COPY ./config/postfix/master.cf /etc/postfix/master.cf
COPY ./config/opendkim/opendkim.conf /etc/opendkim.conf
COPY ./config/opendkim/KeyTable /etc/opendkim/KeyTable
COPY ./config/opendkim/SigningTable /etc/opendkim/SigningTable
COPY ./config/opendkim/TrustedHosts /etc/opendkim/TrustedHosts
COPY ./config/opendkim/keys /etc/opendkim/keys
COPY ./config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

RUN echo 'nameserver 8.8.8.8\nnameserver 8.8.4.4' > /var/spool/postfix/etc/resolv.conf

# Настройка прав
RUN chown -R opendkim:opendkim /etc/opendkim /var/spool/postfix-opendkim/opendkim
RUN chmod -R 600 /etc/opendkim/keys/yakov89.ru/*

CMD ["/usr/bin/supervisord", "-n"]
