FROM postgres:9.4.4

ADD sp_00_generate_functions.sql /docker-entrypoint-initdb.d

USER root
#pre-reuisites
RUN apt-get update && \
    apt-get install -y build-essential git curl unzip zip wget libpq-dev gfortran libatlas-base-dev python-pip python-dev \
    python-lxml python-crypto python-cssselect python-openssl python-w3lib python-twisted libxml2-dev libxslt1-dev \
    zlib1g-dev libffi-dev libssl-dev postgresql-plpython-9.4 \
    fonts-dejavu \
    gcc && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

ADD requirements.txt /tmp/requirements.txt
RUN pip install -r /tmp/requirements.txt

ADD sp_00_generate_functions.sql /data/bd
ADD sp_01_generate_tblMeta.sql /data/bd
ADD sp_02_generate_tblCluster.sql /data/bd
ADD scraped.zip /tmp/scraped.zip
RUN unzip /tmp/scraped.zip -d /data


