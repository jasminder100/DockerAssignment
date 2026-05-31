FROM docker.elastic.co/beats/filebeat:8.12.0

COPY filebeat.yml /usr/share/filebeat/filebeat.yml

# Filebeat requires strict permissions on its config file
USER root
RUN chmod go-w /usr/share/filebeat/filebeat.yml
USER filebeat
