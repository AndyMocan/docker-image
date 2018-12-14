FROM  ubuntu:16.04
MAINTAINER wang-zhu@live.com
USER root
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse' > /etc/apt/sources.list && \
    echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse' >> /etc/apt/sources.list && \
    echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse' >> /etc/apt/sources.list && \
    echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse' >> /etc/apt/sources.list && \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    apt-get update -y && \
    apt-get clean
# 安装 Docker
RUN apt-get -y install apt-transport-https ca-certificates curl software-properties-common && \
    curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | apt-key add - && \
    add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable" && \
    apt-get update -y && \
    apt-get install -y docker-ce
# 安装 Docker Compose
WORKDIR /usr/local/bin
# RUN wget https://raw.githubusercontent.com/topsale/resources/master/docker/docker-compose
COPY docker-compose ./
RUN chmod +x docker-compose

# 安装 Java
RUN mkdir -p /usr/local/java
WORKDIR /usr/local/java
COPY jdk-8u152-linux-x64.tar.gz /usr/local/java
RUN tar -zxvf jdk-8u152-linux-x64.tar.gz && \
    rm -fr jdk-8u152-linux-x64.tar.gz && \
    mv jdk1.8.0_181 jdk1.8.0_152
#安装tomcat
RUN mkdir -p /usr/local/tomcat
WORKDIR /usr/local/tomcat
COPY apache-tomcat-8.5.32.tar.gz /usr/local/tomcat
RUN tar -zxvf apache-tomcat-8.5.32.tar.gz && \
    rm -fr apache-tomcat-8.5.32.tar.gz && \
    mv apache-tomcat-8.5.32/*  ./  && \
    rm -rf  apache-tomcat-8.5.32 && \
    git config --global user.name "wangzhu" && \
    git config --global user.email "wangzhu@originaltek.com"

# 安装 Maven
RUN mkdir -p /usr/local/maven
WORKDIR /usr/local/maven
# RUN wget https://raw.githubusercontent.com/topsale/resources/master/maven/apache-maven-3.5.3-bin.tar.gz
COPY apache-maven-3.5.3-bin.tar.gz /usr/local/maven
RUN tar -zxvf apache-maven-3.5.3-bin.tar.gz && \
    rm -fr apache-maven-3.5.3-bin.tar.gz
#安装sonar-scanner
RUN mkdir -p /usr/local/sonar-scanner
WORKDIR /usr/local/sonar-scanner
ADD sonar-scanner-cli-3.2.0.1227-linux.zip /usr/local/sonar-scanner
RUN apt-get install zip -y && \
    unzip sonar-scanner-cli-3.2.0.1227-linux.zip && \
    rm -rf sonar-scanner-cli-3.2.0.1227-linux.zip && \
    mv sonar-scanner-3.2.0.1227-linux/* ./ && \
    rm -rf  sonar-scanner-3.2.0.1227-linux
# 配置环境变量
ENV JAVA_HOME /usr/local/java/jdk1.8.0_152
ENV MAVEN_HOME /usr/local/maven/apache-maven-3.5.3
ENV CATALINA_BASE /usr/local/tomcat
ENV CATALINA_HOME /usr/local/tomcat
ENV PATH $PATH:$JAVA_HOME/bin:$MAVEN_HOME/bin:$CATALINA_HOME/bin:/usr/local/sonar-scanner/bin
WORKDIR /
CMD ["/usr/local/tomcat/bin/catalina.sh","run"]
