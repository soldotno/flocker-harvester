FROM clusterhq/elasticsearch

USER root
RUN /usr/share/elasticsearch/bin/plugin --install com.github.richardwilly98.elasticsearch/elasticsearch-river-mongodb/2.0.9
RUN /usr/share/elasticsearch/bin/plugin --install elasticsearch/elasticsearch-mapper-attachments/2.5.0
USER elasticsearch
