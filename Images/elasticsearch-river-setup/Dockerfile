FROM ubuntu-debootstrap:14.04
USER root
RUN echo "Will execute: curl ${RIVER_MAPPING} -O mapping.json && curl http://es:9200/_river/${RIVER_NAME}/_meta -d @mapping.json"

CMD apt-get update && apt-get -y install curl && \
    curl ${RIVER_MAPPING} -o mapping.json && \
    curl http://${es}:9200/_river/${RIVER_NAME}/_meta -d @mapping.json
