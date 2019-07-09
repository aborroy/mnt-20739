FROM alfresco/alfresco-search-services:1.3.0.3

# COMMON
ARG ALFRESCO_HOSTNAME
ARG SOLR_HOSTNAME
ENV ALFRESCO_HOSTNAME $ALFRESCO_HOSTNAME
ENV SOLR_HOSTNAME $SOLR_HOSTNAME

# Configure Alfresco Service Name
RUN sed -i '/^bash.*/i sed -i "'"s/alfresco.host=localhost/alfresco.host=${ALFRESCO_HOSTNAME}/g"'" ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties\n' \
    ${DIST_DIR}/solr/bin/search_config_setup.sh && \
    sed -i '/^bash.*/i sed -i "'"s/solr.host=localhost/solr.host=${SOLR_HOSTNAME}/g"'" ${DIST_DIR}/solrhome/conf/shared.properties\n' \
    ${DIST_DIR}/solr/bin/search_config_setup.sh

# COMMS
ARG ALFRESCO_COMMS
ENV ALFRESCO_COMMS $ALFRESCO_COMMS

# Configure SOLR cores to run in HTTPs mode from template
RUN if [ "$ALFRESCO_COMMS" == "https" ] ; then \
    sed -i '/^bash.*/i sed -i "'"s/alfresco.secureComms=none/alfresco.secureComms=https/g"'" ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties\n' \
    ${DIST_DIR}/solr/bin/search_config_setup.sh; \
else \
    sed -i '/^bash.*/i sed -i "'"s/alfresco.secureComms=https/alfresco.secureComms=none/g"'" ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties\n' \
    ${DIST_DIR}/solr/bin/search_config_setup.sh; \
fi

# SHARDING
ARG ENABLE_SHARDING
ARG NUM_SHARDS
ARG SHARD_ID
ARG SHARD_RANGE
ENV ENABLE_SHARDING $ENABLE_SHARDING
ENV NUM_SHARDS $NUM_SHARDS
ENV SHARD_ID $SHARD_ID
ENV SHARD_RANGE $SHARD_RANGE

# Set Port Number and Sharding ID for this Shard Service
RUN if [ "$ENABLE_SHARDING" == "true" ] ; then \
  sed -i '/^bash.*/i echo "\nshard.range=${SHARD_RANGE}\nsolr.port.ssl=8983\nshard.count=${NUM_SHARDS}\nshard.instance=${SHARD_ID}" >> ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties\n' \
  ${DIST_DIR}/solr/bin/search_config_setup.sh && \
  sed -i '/^bash.*/i echo "\nalfresco.port=8080\nalfresco.port.ssl=8443\nalfresco.baseUrl=/alfresco" >> ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties\n' \
  ${DIST_DIR}/solr/bin/search_config_setup.sh && \
  sed -i '/^bash.*/i sed -i "'"s/shard.method=DB_ID/shard.method=DB_ID_RANGE/g"'" ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties\n' \
  ${DIST_DIR}/solr/bin/search_config_setup.sh; \
fi
