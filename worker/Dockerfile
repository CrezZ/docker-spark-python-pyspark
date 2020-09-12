#FROM bde2020/spark-base:3.0.0-hadoop3.2
#FROM bde2020/spark-worker:2.2.0-hadoop2.8-hive-java8
#FROM jfloff/alpine-python:3.6
#FROM alpine:3.11
#FROM python:3.6-alpine3.10
FROM bde2020/hadoop-base:2.0.0-hadoop3.2.1-java8

########### Create base spark image

ENV ENABLE_INIT_DAEMON true
ENV INIT_DAEMON_BASE_URI http://identifier/init-daemon
ENV INIT_DAEMON_STEP spark_master_init

ENV SPARK_VERSION=3.0.0
ENV HADOOP_VERSION=3.2.1

COPY wait-for-step.sh /
COPY execute-step.sh /
COPY finish-step.sh /

#COPY bde-spark.css /css/org/apache/spark/ui/static/timeline-view.css

RUN echo deb http://ftp.de.debian.org/debian testing main >> /etc/apt/sources.list 
#    echo 'APT::Default-Release "stable";' |  tee -a /etc/apt/apt.conf.d/00local && \
     
RUN apt-get update && apt-get -y -t testing install python3.6 python3-pip

#python3 python3-pip

#RUN apt update && apt install -y curl bash openjdk8-jre   \
RUN apt update && apt -t testing install -y curl bash   python3-pip  wget
RUN ln -s /lib64/ld-linux-x86-64.so.2 /lib/ld-linux-x86-64.so.2 \
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

RUN apt install -y  pkg-config libpng-dev libfreetype6-dev   && ln -s /usr/bin/python3 /usr/bin/python

RUN  python3 -m pip install -U --force-reinstall pip
RUN pip3 install kiwisolver &&  pip3 install numpy
RUN pip3 install  matplotlib   
RUN pip3 install scapy pyspark hdfs3

RUN chmod +x /worker.sh

CMD ["/bin/bash", "/worker.sh"]
