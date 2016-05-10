# Inspired from MAINTAINER Carlos Moro kudos to Hypriot <dordoka@gmail.com>
# Pull base image
FROM resin/raspberrypi3-debian:jessie
MAINTAINER Kalyan

# Default to UTF-8 file.encoding
ENV LANG C.UTF-8
ENV TOMCAT_VERSION 8.0.33

# Install dependencies
RUN echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu precise main" | tee -a /etc/apt/sources.list \
    && echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu precise main" | tee -a /etc/apt/sources.list \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys EEA14886 \
    && apt-get update \
    && echo oracle-java7-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections \
    && apt-get install -y --no-install-recommends \
               curl \ 
               vim \
               oracle-java7-installer \
    && rm -rf /var/lib/apt/lists/*

# Define commonly used JAVA_HOME variable
ENV JAVA_HOME /usr/lib/jvm/java-7-oracle

# Get Tomcat
RUN wget --quiet --no-cookies http://apache.rediris.es/tomcat/tomcat-8/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz -O /tmp/tomcat.tgz

# Uncompress
RUN tar xzvf /tmp/tomcat.tgz -C /opt \
    && mv /opt/apache-tomcat-${TOMCAT_VERSION} /usr/local/tomcat \
    && rm /tmp/tomcat.tgz

# Remove garbage
RUN rm -rf /usr/local/tomcat/webapps/examples \
 && rm -rf /usr/local/tomcat/webapps/docs \
 && rm -rf /usr/local/tomcat/webapps/ROOT

ENV CATALINA_HOME /usr/local/tomcat/
ENV PATH $PATH:$CATALINA_HOME/bin 

WORKDIR /usr/local/tomcat/
EXPOSE 8080

# Launch Tomcat
CMD ["/usr/local/tomcat/bin/catalina.sh", "run"]
