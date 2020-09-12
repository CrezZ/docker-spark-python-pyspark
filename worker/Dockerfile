#FROM bde2020/spark-base:3.0.0-hadoop3.2
#FROM bde2020/spark-worker:2.2.0-hadoop2.8-hive-java8
#FROM jfloff/alpine-python:3.6
#FROM alpine:3.11
FROM python:3.6-alpine3.10

########### Create base spark image

ENV ENABLE_INIT_DAEMON true
ENV INIT_DAEMON_BASE_URI http://identifier/init-daemon
ENV INIT_DAEMON_STEP spark_master_init

ENV SPARK_VERSION=3.0.0
ENV HADOOP_VERSION=3.2

COPY wait-for-step.sh /
COPY execute-step.sh /
COPY finish-step.sh /

#COPY bde-spark.css /css/org/apache/spark/ui/static/timeline-view.css

RUN apk update && apk add --no-cache curl bash openjdk8-jre python3 py-pip nss libc6-compat \
      && ln -s /lib64/ld-linux-x86-64.so.2 /lib/ld-linux-x86-64.so.2 \
      && chmod +x *.sh \
      && wget https://archive.apache.org/dist/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz \
      && tar -xvzf spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz \
      && mv spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION} spark \
      && rm spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz \
      #&& cd /css \
      #&& jar uf /spark/jars/spark-core_2.11-${SPARK_VERSION}.jar org/apache/spark/ui/static/timeline-view.css \
      && cd /

#Give permission to execute scripts
RUN chmod +x /wait-for-step.sh && chmod +x /execute-step.sh && chmod +x /finish-step.sh

# Fix the value of PYTHONHASHSEED
# Note: this is needed when you use Python 3.3 or greater
ENV PYTHONHASHSEED 1

################################# Create worker image

COPY worker.sh /

ENV SPARK_WORKER_WEBUI_PORT 8081
ENV SPARK_WORKER_LOG /spark/logs
ENV SPARK_MASTER "spark://spark-master:7077"

EXPOSE 8081

##############################  Install pyspark


#RUN apk add  pkg-config libpng12-dev libfreetype6-dev  && ln -s /usr/bin/python3 /usr/bin/python
RUN apk add  gcc musl-dev g++ make alpine-sdk
RUN pip3 install kiwisolver &&  pip3 install numpy
RUN apk add zlib-dev libjpeg jpeg-dev  && python3 -m pip install -U --force-reinstall pip
RUN pip3 install  matplotlib   
RUN pip3 install scapy pyspark hdfs3

CMD ["/bin/bash", "/worker.sh"]
