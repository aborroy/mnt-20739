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
	&& mkdir -p $CATALINA_HOME/conf/Catalina/localhost $CATALINA_HOME/shared/classes/alfresco/extension $CATALINA_HOME/shared/lib $ALF_HOME/modules/platform \
	&& mv $DIST/web-server/conf/Catalina/localhost/alfresco.xml tomcat/conf/Catalina/localhost/ \
        && mv $DIST/web-server/lib/*.jar tomcat/lib/ \
        && mv $DIST/web-server/webapps/alfresco.war tomcat/webapps/ \
        && mv $DIST/alf_data . \
        && mv $DIST/amps . \
        && mv $DIST/bin . \
        && mv $DIST/licenses . \
        && mv $DIST/README.txt . \
        && rm -rf $CATALINA_HOME/webapps/docs \
        && rm -rf $CATALINA_HOME/webapps/examples \
        && rm -rf $DIST

COPY assets/tomcat/catalina.properties tomcat/conf/catalina.properties
COPY assets/tomcat/setenv.sh tomcat/bin/setenv.sh
COPY assets/tomcat/alfresco.xml tomcat/conf/Catalina/localhost/alfresco.xml
COPY assets/alfresco/alfresco-global.properties tomcat/shared/classes/alfresco-global.properties

# alfresco-pdf-renderer
RUN set -x \
	&& mkdir /usr/local/alfresco/alfresco-pdf-renderer \
	&& wget https://artifacts.alfresco.com/nexus/service/local/repositories/releases/content/org/alfresco/alfresco-pdf-renderer/1.0/alfresco-pdf-renderer-1.0-linux.tgz \
	&& tar xzvf alfresco-pdf-renderer-1.0-linux.tgz -C /usr/local/alfresco/alfresco-pdf-renderer \
	&& rm -f alfresco-pdf-renderer-1.0-linux.tgz

ENV PATH $ALF_HOME/bin:$PATH
ENV LANG es_ES.utf8

# Add license
ADD assets/lic $CATALINA_HOME/shared/classes/alfresco/extension/license/

# Apply Search Services
RUN bash ./bin/apply_amps.sh -nobackup

EXPOSE 8080 8009
VOLUME $ALF_HOME/alf_data
CMD ["catalina.sh", "run"]
