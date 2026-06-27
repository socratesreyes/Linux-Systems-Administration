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
