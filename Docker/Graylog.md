# Graylog

Graylog is a centralized logging solution that allows users to aggregate and search logs. It provides a powerful query language, a processing pipeline for data transformation, alerting abilities, and much more. It is fully extensible through a REST API. Add-ons can be downloaded from the Graylog Marketplace⁠.

## To run Graylog in Docker on a Windows host

1. Create a Docker Network (optional but recommended):

```bash
PS> docker network create graylog_network
```

2. Run Elasticsearch in Docker: Graylog requires Elasticsearch to store log data, so we need to run Elasticsearch as a container.

```bash
PS> docker run --name elasticsearch --network graylog_network -d \
  -e "discovery.type=single-node" \
  -e "ES_JAVA_OPTS=-Xms1g -Xmx1g" \
  -p 9200:9200 \
  docker.elastic.co/elasticsearch/elasticsearch:7.10.0
```

3. Run MongoDB in Docker: Graylog also uses MongoDB to store configuration data, so you’ll need MongoDB running in a container.

```bash
docker run --name mongo --network graylog_network -d \
  -p 27017:27017 \
  mongo:4.2
```

4. Run Graylog in Docker: Now, we can run the Graylog container itself, linking it with Elasticsearch and MongoDB.

```bash
docker run --name graylog --network graylog_network -d \
  -e GRAYLOG_HTTP_EXTERNAL_URI=http://localhost:9000/ \
  -e GRAYLOG_ROOT_PASSWORD_SHA2=<your-sha2-password> \
  -e GRAYLOG_MONGO_URI=mongodb://mongo/graylog \
  -e GRAYLOG_ELASTICSEARCH_HOSTS=http://elasticsearch:9200 \
  -p 9000:9000 \
  graylog/graylog:6.3
```

5. Access Graylog: After starting the containers, you can access the Graylog web interface by navigating to `http://localhost:9000` in your web browser.

## To run Graylog in Docker on a Windows host with Docker Compose:

By default, docker compose (or docker-compose on older versions) only looks for `docker-compose.yml` (and `docker-compose.override.yml`).
If you have multiple `.yaml` or `.yml` files with different names in the same directory, you need to specify them with `-f` (or `--file`).

```bash
docker compose -f myfile.yaml up
docker compose -p project-name -f myfile.yaml up

# You can stack them together (later files override earlier ones):
docker compose -f base.yaml -f override.yaml up
```

Create a `docker-compose.yml` file

```bash
echo. > docker-compose.yml
type nul > docker-compose.yml
ni docker-compose.yml
```

Edit the file as follows:

```yaml
services:
  mongodb:
    image: mongo:4.2
    restart: always
    volumes:
      - mongo_data:/data/db
    networks:
      - graylog_network
    
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.10.0
    environment:
      - discovery.type=single-node
      - ES_JAVA_OPTS=-Xms1g -Xmx1g
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - elasticsearch_data:/usr/share/elasticsearch/data
    networks:
      - graylog_network
    ports:
      - "9200:9200"

  graylog:
    image: graylog/graylog:4.2
    environment:
      - GRAYLOG_HTTP_EXTERNAL_URI=http://localhost:9000/
      - GRAYLOG_ROOT_PASSWORD_SHA2=<your-sha2-password>
      - GRAYLOG_MONGO_URI=mongodb://mongodb/graylog
      - GRAYLOG_ELASTICSEARCH_HOSTS=http://elasticsearch:9200
    ports:
      - "9000:9000"
    depends_on:
      - mongodb
      - elasticsearch
    networks:
      - graylog_network
    restart: always

networks:
  graylog_network:
    driver: bridge

volumes:
  mongo_data:
    driver: local
  elasticsearch_data:
    driver: local
```

Once you’ve created the docker-compose.yml file, open a terminal (PowerShell or Command Prompt) in the same directory and run:

```bash
PS> docker compose -p graylog-stack -f .\docker-compose-graylog.yaml up -d

# Deploy the Graylog application with Docker Compose
# [-d] (to have it running in the background)
docker-compose up -d -p myproject

# Graylog Web Interface: http://localhost:9000

# To check the status of your containers
docker compose ps

# To stop the containers:
docker-compose down

# To restart the containers:
docker-compose restart

# View your Docker logs to obtain your initial login credentials
docker compose logs graylog

# To display the logs of all services defined in your Graylog environment and verify they are running as intended, use the following command:
docker compose logs -f
```

## Creating a SHA256 Password Hash

```bash
# LINUX
echo -n "MySecretPassword" | shasum -a 256

# WINDOWS
$password = "aA123456"
$bytes = [System.Text.Encoding]::UTF8.GetBytes($password)
$hash = [System.Security.Cryptography.SHA256]::Create().ComputeHash($bytes)
$hashString = ($hash | ForEach-Object { $_.ToString("x2") }) -join ""
# Display the hash
$hashString

aA123456 -> 1967bfa68e40013348762184193c2a457be7a6975d1d5da6a8161d0e9165dbbb
```