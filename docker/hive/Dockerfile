FROM ubuntu:latest

ENV JAVA_HOME=/usr/lib/jvm/default-java
ENV HADOOP_HOME=/opt/hadoop
ENV HIVE_HOME=/opt/hive

WORKDIR /opt

RUN apt-get update && apt-get install -y wget

RUN apt-get install -y default-jre

RUN wget https://downloads.apache.org/hive/hive-2.3.9/apache-hive-2.3.9-bin.tar.gz && \
    tar -xzvf apache-hive-2.3.9-bin.tar.gz && \
    mv apache-hive-2.3.9-bin hive && \ 
    rm -f apache-hive-2.3.9-bin.tar.gz

RUN wget https://downloads.apache.org/hadoop/common/hadoop-3.3.1/hadoop-3.3.1.tar.gz && \
    tar -xzvf hadoop-3.3.1.tar.gz && \
    mv hadoop-3.3.1 hadoop && \
    rm -f hadoop-3.3.1.tar.gz
    
COPY conf ${HIVE_HOME}/conf

Copy driverdb/mysql-connector-java-8.0.26.jar /opt/hive/lib
Copy driverdb/ojdbc8.jar /opt/hive/lib
Copy driverdb/mssql-jdbc-9.4.0.jre8.jar /opt/hive/lib
Copy driverdb/mssql-jdbc-9.4.0.jre11.jar /opt/hive/lib
Copy driverdb/mssql-jdbc-9.4.0.jre16.jar /opt/hive/lib

RUN groupadd -r hive --gid=1000 && \
    useradd -r -g hive --uid=1000 -d ${HIVE_HOME} hive && \
    chown hive:hive -R ${HIVE_HOME}

USER hive
WORKDIR ${HIVE_HOME}
EXPOSE 9083

ENTRYPOINT ["bin/hive"]
CMD ["--service","metastore"]
