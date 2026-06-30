LAB 1: Basic Container Management
Step 1: Verify Docker is Running
bash
# On Laptop 1 (Ubuntu WSL)
docker --version
sudo systemctl status docker
docker ps -a
Expected output: Docker version displayed, service running, and a list of any existing containers.

Step 2: Pull and Run a Test Container
bash
# Pull the Nginx image
docker pull nginx:latest

# Run a test container
docker run -d --name test-nginx -p 8080:80 nginx

# Verify it's running
docker ps -a

# Test it's accessible
curl http://localhost:8080
Expected output: You should see the Nginx welcome page in the curl response.

Step 3: Inspect Container Logs
bash
# View logs
docker logs test-nginx

# Follow logs in real-time (press Ctrl+C to stop)
docker logs -f test-nginx

# Access the container shell
docker exec -it test-nginx bash

# Inside the container, explore
ls -la
cat /etc/nginx/nginx.conf
exit  # Exit the container

📋 LAB 2: Mounting Volumes


Step 1: Create a Custom HTML Page
bash
# Create a directory for our custom files
mkdir -p ~/docker-lab/nginx-html
cd ~/docker-lab/nginx-html

# Create a custom index.html
cat > index.html << 'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>My Custom Nginx</title>
</head>
<body>
    <h1>Hello from My Custom Nginx Container!</h1>
    <p>This is served from a mounted volume.</p>
</body>
</html>
EOF


Step 2: Run Container with Mounted Volume
bash
# Stop and remove the existing container
docker stop test-nginx
docker rm test-nginx

# Run new container with volume mount
docker run -d --name custom-nginx -p 8080:80 \
  -v ~/docker-lab/nginx-html:/usr/share/nginx/html \
  nginx

# Test it
curl http://localhost:8080


Expected output: You should see your custom HTML page!

📋 LAB 3: Database Container with Persistent Volume
Step 1: Create a MySQL Container with Volume
bash
# Create a persistent volume for MySQL data
mkdir -p ~/docker-lab/mysql-data

# Run MySQL container with mounted volume
docker run -d --name test-mysql \
  -e MYSQL_ROOT_PASSWORD=securepassword123 \
  -v ~/docker-lab/mysql-data:/var/lib/mysql \
  mysql:5.7

# Check it's running
docker ps | grep mysql
Step 2: Connect to the Database and Create Data
bash
# Access MySQL container
docker exec -it test-mysql bash

# Inside container, connect to MySQL
mysql -uroot -psecurepassword123

# Create a test database
CREATE DATABASE testdb;
USE testdb;
CREATE TABLE users (id INT, name VARCHAR(50));
INSERT INTO users VALUES (1, 'John Doe');
SELECT * FROM users;
EXIT;  # Exit MySQL
EXIT;  # Exit container


Step 3: Verify Data Persistence
bash
# Stop and remove the container
docker stop test-mysql
docker rm test-mysql

# Create a new container with the same volume
docker run -d --name new-mysql \
  -e MYSQL_ROOT_PASSWORD=securepassword123 \
  -v ~/docker-lab/mysql-data:/var/lib/mysql \
  mysql:5.7

# Check if data still exists
docker exec -it new-mysql mysql -uroot -psecurepassword123 -e "SELECT * FROM testdb.users;"
Expected output: You should see "John Doe" - proving data persisted!

📋 LAB 4: Docker Networking
Step 1: Create a Custom Network
bash
# Create a custom network
docker network create my-app-network

# Verify networks
docker network ls
Step 2: Run Containers on the Same Network
bash
# Run Nginx on the network
docker run -d --name web-app \
  --network my-app-network \
  -p 8080:80 \
  nginx

# Run MySQL on the same network
docker run -d --name db \
  --network my-app-network \
  -e MYSQL_ROOT_PASSWORD=securepassword123 \
  -e MYSQL_DATABASE=myapp \
  mysql:5.7

# Test connectivity from web-app to db

##################
install ping in docker
docker exec -u 0 web-app apt-get update && docker exec -u 0 web-app apt-get install -y iputils-ping
#install nslookup
docker exec -it -u root web-app apt-get install -y dnsutils


#####
docker exec web-app ping db
docker exec web-app nslookup db
📋 LAB 5: Docker Compose
Step 1: Create a Docker Compose File
bash
cd ~/docker-lab/
cat > docker-compose.yml << 'EOF'
version: '3.8'

services:
  web:
    image: nginx
    ports:
      - "8080:80"
    volumes:
      - ./nginx-html:/usr/share/nginx/html
    networks:
      - app-network

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: securepassword123
      MYSQL_DATABASE: myapp
    volumes:
      - ./mysql-data:/var/lib/mysql
    networks:
      - app-network

  adminer:
    image: adminer
    ports:
      - "8081:8080"
    networks:
      - app-network

networks:
  app-network:
    driver: bridge
EOF
Step 2: Start the Stack
bash
# Start all services in the background
docker-compose up -d

# Check status
docker-compose ps

# View logs
docker-compose logs web


###############################################
##LAB 2 Set up a complete LAMP stack with Docker Compose

#Install and configure Kind (Kubernetes in Docker)

#Deploy your first pod, deployment, and service#


Create a Complete LAMP Stack with Docker Compose
bash
cd ~/docker-lab/

# Create a more complete docker-compose.yml
cat > docker-compose-full.yml << 'EOF'
version: '3.8'

services:
  # Web Server
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./nginx-html:/usr/share/nginx/html
      - ./nginx-conf:/etc/nginx/conf.d
    depends_on:
      - php
      - db
    networks:
      - app-network

  # PHP-FPM
  php:
    image: php:8.1-fpm
    volumes:
      - ./nginx-html:/var/www/html
    networks:
      - app-network

  # Database
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: securepassword123
      MYSQL_DATABASE: myapp
      MYSQL_USER: appuser
      MYSQL_PASSWORD: apppassword123
    volumes:
      - ./mysql-data:/var/lib/mysql
    networks:
      - app-network
    ports:
      - "3306:3306"

  # phpMyAdmin
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    environment:
      PMA_HOST: db
      PMA_PORT: 3306
    ports:
      - "8081:80"
    depends_on:
      - db
    networks:
      - app-network

  # Redis Cache
  redis:
    image: redis:alpine
    ports:
      - "6379:6379"
    networks:
      - app-network

  # Prometheus (for monitoring)
  prometheus:
    image: prom/prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      - ./prometheus-data:/prometheus
    ports:
      - "9090:9090"
    networks:
      - app-network

  # Grafana (for dashboards)
  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
    volumes:
      - ./grafana-data:/var/lib/grafana
    networks:
      - app-network

networks:
  app-network:
    driver: bridge
EOF



########### Create the Prometheus Configuration

cat > prometheus.yml << 'EOF'
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets: ['localhost:9100']

  - job_name: 'docker'
    static_configs:
      - targets: ['localhost:9323']
EOF



