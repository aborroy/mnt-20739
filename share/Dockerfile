FROM keensoft/base:centos7-openjdk8-tomcat7

RUN set -x \
        && yum install -y \
                unzip \
                wget \
                ImageMagick \
                ghostscript \
        && yum clean all

ENV ALF_HOME /usr/local/alfresco
ENV ALF_DOWNLOAD_URL https://releases.alfresco.com/Enterprise-5.2/5.2.3/build-00012/ALL/alfresco-content-services-distribution-5.2.3.zip

RUN set -x \
        && mkdir -p $ALF_HOME \
        && wget --no-check-certificate $ALF_DOWNLOAD_URL \
        && unzip alfresco-content-services-distribution-*.zip -d /tmp \
        && rm -f alfresco-content-services-distribution-*.zip

WORKDIR $ALF_HOME
ENV DIST /tmp/alfresco-content-services-distribution-5.2.3
ENV PATH $ALF_HOME/bin:$PATH

ENV ALF_HOME /usr/local/alfresco
ENV CATALINA_HOME /usr/local/alfresco/tomcat
WORKDIR $ALF_HOME

# basic configuration
RUN set -x \
        && ln -s /usr/local/tomcat /usr/local/alfresco/tomcat \
        && mkdir -p $CATALINA_HOME/conf/Catalina/localhost $CATALINA_HOME/shared/classes/alfresco/web-extension $CATALINA_HOME/shared/lib $ALF_HOME/modules/share \
        && mv $DIST/web-server/conf/Catalina/localhost/share.xml tomcat/conf/Catalina/localhost/ \
        && mv $DIST/web-server/lib/*.jar tomcat/lib/ \
        && mv $DIST/web-server/webapps/share.war tomcat/webapps/ \
        && mkdir $ALF_HOME/amps_share \
        && mv $DIST/bin . \
        && mv $DIST/licenses . \
        && mv $DIST/README.txt . \
        && rm -rf $CATALINA_HOME/webapps/docs \
        && rm -rf $CATALINA_HOME/webapps/examples \
        && rm -rf $DIST

COPY assets/tomcat/catalina.properties tomcat/conf/catalina.properties
COPY assets/tomcat/setenv.sh tomcat/bin/setenv.sh
COPY assets/tomcat/share.xml tomcat/conf/Catalina/localhost/share.xml
COPY assets/share/share-config-custom.xml tomcat/shared/classes/alfresco/web-extension/share-config-custom.xml

ENV PATH $ALF_HOME/bin:$PATH

RUN useradd -ms /bin/bash alfresco
RUN set -x && chown -RL alfresco:alfresco $ALF_HOME
USER alfresco

EXPOSE 8080 8009
CMD ["catalina.sh", "run"]
