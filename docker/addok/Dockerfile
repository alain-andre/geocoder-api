FROM python:3.4

LABEL maintainer="Mapotempo <contact@mapotempo.com>"

ARG ADDOK_VERSION
ENV ADDOK_VERSION ${ADDOK_VERSION:-1.0.0}

RUN apt-get -q update && \
    apt-get -qy install redis-tools && \
    \
    pip install gunicorn addok==${ADDOK_VERSION} addok-fr==${ADDOK_VERSION} addok-france==${ADDOK_VERSION} addok-csv==${ADDOK_VERSION} && \
    pip install git+https://github.com/Mapotempo/addok_france_clean && \
    pip install git+https://github.com/Mapotempo/addok_search2steps && \
    pip install git+https://github.com/Mapotempo/addok_usage_name_BAN_FR && \
    mkdir /etc/addok && \
    \
# Cleanup Debian packages
    apt-get remove -y git build-essential zlib1g-dev gdal-bin && \
    apt-get autoremove -y && \
    apt-get clean && \
    echo -n > /var/lib/apt/extended_states && \
    rm -rf /tmp/* /var/lib/apt/lists/* /var/tmp/*

ADD config.py /etc/addok/config.py

EXPOSE 7878

ENTRYPOINT ["gunicorn"]

CMD ["--bind", "127.0.0.1:7878", "--env", "ADDOK_CONFIG_MODULE=/etc/addok/config.py", "--timeout", "7200", "--workers", "8", "addok.http.wsgi"]