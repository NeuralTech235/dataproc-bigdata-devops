FROM ubuntu:latest

ENV JAVA_HOME=/usr/lib/jvm/default-java
ENV HADOOP_HOME=/opt/hadoop
ENV HIVE_HOME=/opt/hive

WORKDIR /opt

RUN apt-get update && apt-get install -y wget

RUN apt-get install -y default-jre

RUN apt-get install -y vim

RUN wget -q https://downloads.apache.org/hive/hive-2.3.9/apache-hive-2.3.9-bin.tar.gz && \
    tar -xzvf apache-hive-2.3.9-bin.tar.gz && \
    mv apache-hive-2.3.9-bin hive && \ 
    rm -f apache-hive-2.3.9-bin.tar.gz

COPY conf ${HIVE_HOME}/conf
COPY driverdb/mysql-connector-java-8.0.26.jar /opt/hive/lib
COPY driverdb/ojdbc8.jar /opt/hive/lib
COPY driverdb/mssql-jdbc-9.4.0.jre8.jar /opt/hive/lib
COPY driverdb/mssql-jdbc-9.4.0.jre11.jar /opt/hive/lib
COPY driverdb/mssql-jdbc-9.4.0.jre16.jar /opt/hive/lib

RUN wget -q https://downloads.apache.org/hadoop/common/hadoop-3.3.1/hadoop-3.3.1.tar.gz && \
    tar -xzvf hadoop-3.3.1.tar.gz && \
    mv hadoop-3.3.1 hadoop && \
    rm -f hadoop-3.3.1.tar.gz

COPY adls2libs/azure-data-lake-store-sdk-2.3.9.jar /opt/hadoop/share/hadoop/tools/lib/
COPY adls2libs/hadoop-azure-datalake-3.3.1.jar /opt/hadoop/share/hadoop/tools/lib/
COPY hadoop/core-site.xml /opt/hadoop/etc/hadoop/core-site.xml

RUN groupadd -r hadoop --gid=1001 && \
    useradd -r -g hadoop --uid=1001 -d ${HADOOP_HOME} hadoop && \
    chown hadoop:hadoop -R ${HADOOP_HOME}

RUN groupadd -r hive --gid=1000 && \
    useradd -r -g hive --uid=1000 -d ${HIVE_HOME} hive && \
    chown hive:hive -R ${HIVE_HOME}

USER hive
WORKDIR ${HIVE_HOME}
EXPOSE 9083
EXPOSE 10000
EXPOSE 10001
EXPOSE 10002

ENTRYPOINT ["bin/hive"]
CMD ["--service","metastore"]