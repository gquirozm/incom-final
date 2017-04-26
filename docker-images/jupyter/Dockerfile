# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
FROM jupyter/pyspark-notebook

MAINTAINER INCOM Project <radianstk@gmail.com>

USER root

# RSpark config
ENV R_LIBS_USER $SPARK_HOME/R/lib

# pre-requisites
#RUN apt-get update && \
#    apt-get install -y build-essential git curl libpq-dev gfortran libatlas-base-dev python-pip python-dev \
#    python-lxml python-crypto python-cssselect python-openssl python-w3lib python-twisted libxml2-dev libxslt1-dev \
#    zlib1g-dev libffi-dev libssl-dev \
#    fonts-dejavu \
#    gcc && apt-get clean && \
#    rm -rf /var/lib/apt/lists/*

RUN apt-get update && \
    apt-get install -y build-essential git curl libpq-dev gfortran libatlas-base-dev python-pip python-dev \
    fonts-dejavu \
    gcc && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

ADD requirements.txt /tmp/requirements.txt
RUN pip install -r /tmp/requirements.txt

USER jovyan
ADD notebook.zip /tmp/notebook.zip
RUN mkdir /home/jovyan/work/data
RUN unzip /tmp/notebook.zip -d /home/jovyan/work

#Ejecuta para automatizar
#CMD ["./algo.sh"]
