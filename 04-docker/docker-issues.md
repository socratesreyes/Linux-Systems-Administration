basic issues with solutions

########
# docker compose

This error indicates that Docker is trying to download the adminer image using an IPv6 address,

~/docker-lab$ docker compose up -d
WARN[0000] /home/soc/docker-lab/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
[+] up 16/19
 ⠇ Image adminer [⣿⣿⡀⣿⣿⣿⣿⣿⠀⣿⣿⣿⣿⣿⣿⣿⣿⣿] 16.01MB / 46.03MB Pulling                                                                                         189.8s
failed to copy: read tcp [2405:8d40:4878:8408:7565:49f:42b3:e5cc]:58729->[2600:9000:289a:fe00:9:4855:aac0:93a1]:443: read: network is unreachable


## SOLUTION

Force your DNS to use IPv4

bash
sudo nano /etc/resolv.conf
#replace
nameserver 8.8.8.8

#then run 
docker compose up -d

####################################

##port 8080 is in use

Error response from daemon: failed to set up container networking: driver failed programming external connectivity on endpoint
 docker-lab-web-1 (b4a851f7e08dbdbd0ad45f6e863f77cda9028c050b92f7912af541eb87edfb4e): Bind for 0.0.0.0:8080 failed: port is already allocated
SOLUTION

#FILTER COMMAND IDENTIFY WHICH CONTAINER
docker ps -a --filter "publish=8080"

##STOP TEMP THE CONTAINER

docker stop <old_container_name_or_id>

docker compose up -d




###########


+] up 6/28
 ⠴ Image redis:alpine                                  Pulling                                                                                           13.5s
 ✘ Image nginx:alpine                                  Error failed to resolve reference "docker.io/library/nginx:alpine": failed to authorize:...       13.5s
 ! Image prom/prometheus                               Interrupted                                                                                       13.5s
 ⠴ Image mysql:8.0                                     Pulling                                                                                           13.5s
 ! Image php:8.1-fpm                                   Interrupted                                                                                       13.5s
 ⠴ Image phpmyadmin/phpmyadmin [⣿⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⣿⠀⠀⠀⠀⠀] Pulling                                                                                           13.5s
 ! Image grafana/grafana                               Interrupted                                                                                       13.5s
Error response from daemon: failed to resolve reference "docker.io/library/nginx:alpine": failed to authorize: failed to fetch anonymous token: Get "https://auth.docker.io/token?scope=repository%3Alibrary%2Fnginx%3Apull&service=registry.docker.io": net/http: TLS handshake timeout
