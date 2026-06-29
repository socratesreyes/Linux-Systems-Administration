# Docker Cheat
## 🔧 Daily Commands
| Command | Purpose |
|---------|---------|
| `docker ps -a` | List all containers |
| `docker logs -f <id>` | Follow logs |
| `docker exec -it <id> bash` | Enter container |
| `docker stop <id>` | Stop container |
| `docker rm -f <id>` | Remove container |
| `docker system prune -a -f` | Cleanup unused |

## 📂 Important Paths
| Path | Purpose |
|------|---------|
| `/var/lib/docker/` | Docker storage |
| `/etc/docker/daemon.json` | Daemon config |

## 🐛 Common Issues
### Issue: Permission denied
**Fix:** `sudo usermod -aG docker $USER && newgrp docker`

### Issue: No space left
**Fix:** `docker system df` then `docker system prune -a -f`

## 🎯 Interview Prep
- Managed container lifecycle
- Used Docker Compose for multi-container apps
- Understood volumes and networks




#Basic Container Management

docker --version
sudo systemctl status docker
docker ps -a
##########################################

#add user and group
#permission issue security group. user dont have permission
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker

##################
#PULL AND RUN A TEST CONTAINER

# Pull the Nginx image
docker pull nginx:latest

# Run a test container
docker run -d --name test-nginx -p 8080:80 nginx

# Verify it's running
docker ps -a

# Test it's accessible
curl http://localhost:8080

#####################################################
: Inspect Container Logs

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


