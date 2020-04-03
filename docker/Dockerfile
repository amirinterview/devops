FROM centos:8

ENV VERSION=9.0.33 JAVA_HOME=/media/pkgs/jdk-12.0.1 PATH=$PATH:$JAVA_HOME/bin

RUN yum -y update &&\
    yum install -y wget gcc make zlib-devel sudo &&\
    yum clean all &&\
    rm -rf /var/cache/yum

RUN useradd cliuser &&\
    echo "cliuser ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

RUN wget https://www.python.org/ftp/python/3.6.9/Python-3.6.9.tgz -P /tmp &&\
    tar xzf /tmp/Python-3.6.9.tgz -C /usr/src &&\
    cd /usr/src/Python-3.6.9 &&\
    ./configure --enable-optimizations &&\
    make altinstall

ADD ./openjdk-12.0.1_linux-x64_bin.tar.gz /media/pkgs/

RUN wget https://archive.apache.org/dist/tomcat/tomcat-9/v${VERSION}/bin/apache-tomcat-${VERSION}.tar.gz -P /tmp &&\
    useradd -m -U -d /opt/tomcat -s /bin/false tomcat &&\
    tar -xf /tmp/apache-tomcat-${VERSION}.tar.gz -C /opt/tomcat/ &&\
    ln -s /opt/tomcat/apache-tomcat-${VERSION} /opt/tomcat/latest &&\
    chown -R tomcat: /opt/tomcat &&\
    chmod +x /opt/tomcat/latest/bin/*.sh
    
CMD ["/opt/tomcat/latest/bin/catalina.sh", "run"]
