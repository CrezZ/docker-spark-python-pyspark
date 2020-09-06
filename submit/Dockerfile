FROM bde2020/spark-worker:2.2.0-hadoop2.8-hive-java8

#LABEL maintainer="Gezim Sejdiu <g.sejdiu@gmail.com>, Giannis Mouchakis <gmouchakis@gmail.com>"

ENV SPARK_MASTER_NAME spark-master
ENV SPARK_MASTER_PORT 7077
ENV SPARK_APPLICATION_JAR_LOCATION /app/application.jar
ENV SPARK_APPLICATION_PYTHON_LOCATION /app/app.py
ENV SPARK_APPLICATION_MAIN_CLASS my.main.Application
ENV SPARK_APPLICATION_ARGS ""

RUN sed -i 's/ftp.*jessie-backports/archive.debian.org\/debian jessie-backports/g' /etc/apt/sources.list
RUN echo 'Acquire::Check-Valid-Until no;' > /etc/apt/apt.conf.d/99no-check-valid-until
RUN apt-get update 
RUN apt install -y pkg-config libpng12-dev libfreetype6-dev python3 python3-pip && ln -s /usr/bin/python3 /usr/bin/python
RUN pip3 install kiwisolver==1.1.0
RUN pip3 install numpy==1.15
RUN pip3 install  matplotlib==2.2.5  pyspark

