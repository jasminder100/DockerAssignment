# Nginx Log Monitoring with ELK Stack
This project sets up a complete log monitoring pipeline using Docker. It captures Nginx web server logs in real time, ships them to Elasticsearch using Filebeat, and displays them visually in Kibana. The entire stack runs locally with a single Docker Compose command, making it easy to set up and tear down.
# Use Cases
This kind of setup is commonly used in production environments to monitor web server health, track traffic patterns, debug errors, and set up alerts. For example, you can use Kibana to see which URLs are being hit the most, spot a sudden spike in 404 errors, or identify slow requests — all from a single dashboard.
# How It Works
Nginx writes access and error logs to a directory that is mounted as a shared Docker volume. Filebeat watches that volume for new log entries and ships them in real time to an Elasticsearch index (filebeat-*). Kibana then connects to Elasticsearch and lets you search, filter, and visualize those logs through a web interface.
# Prerequisites
1) Docker
2) Docker Compose
3) Minimum 4GB RAM allocated to Docker
# Project Structure
elk-nginx/
├── docker-compose.yml          # Ties all services together
├── nginx/
│   └── Dockerfile              # Nginx with real log files (symlinks removed)
├── elasticsearch/
│   └── Dockerfile              # Elasticsearch single-node setup
├── filebeat/
│   ├── Dockerfile              # Filebeat image
│   └── filebeat.yml            # Config to read logs and ship to Elasticsearch
├── kibana/
│   └── Dockerfile              # Kibana image
└── logs/
    ├── nginx-access.log        # Sample Nginx access logs
    └── nginx-error.log         # Sample Nginx error logs
# Setup and Running
Clone the repository and navigate into the project folder.
git clone https://github.com/YOUR_USERNAME/elk-nginx.git
cd elk-nginx
Start all four services in detached mode.
docker-compose up -d --build
Generate some Nginx traffic so logs are created.
curl http://localhost
curl http://localhost
curl http://localhost
Reset the Elasticsearch password (required on first run with Elasticsearch 8.x).
docker exec -it elasticsearch bin/elasticsearch-reset-password -u elastic
Save the generated password. Then generate a Kibana service account token, as Elasticsearch 8.x does not allow the elastic superuser to be used directly by Kibana.
curl -u 'elastic:<your-password>' -X POST "http://localhost:9200/_security/service/elastic/kibana/credential/token/kibana_token"
Copy the value from the response and add it to the Kibana service in docker-compose.yml.
environment:
  - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
  - ELASTICSEARCH_SERVICEACCOUNTTOKEN=<paste-token-here>
Restart Kibana to apply the changes.
docker-compose up -d kibana
