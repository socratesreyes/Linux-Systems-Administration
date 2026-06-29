REDHAT -- CENTOS
#####################  MANAGEMENT
cat /etc/centos-release  	or hostnamectl		#centos version check




######################
#####	FIREWALL REDHAT - UBUNTU
sudo ss -tulpn | grep :9100		# check if port 9100 is available
########
#open incoming port 9100
sudo firewall-cmd --add-port=9100/tcp --permanent
sudo firewall-cmd --reload


sudo firewall-cmd --list-all		#check available ports open or allowed
sudo firewall-cmd --query-port=9100/tcp

#Standard Rule: "Open Port 9100 for the entire internet."Rich Rule: "Open Port 9100, BUT ONLY if the traffic is coming from my Prometheus Server IP address. Block everyone else."
sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="192.168.1.50" port port="9100" protocol="tcp" accept'
sudo firewall-cmd --reload
sudo firewall-cmd --permanent --remove-rich-rule='rule family="ipv4" source address="192.168.1.50" port port="9100" protocol="tcp" accept'		#remove

###########################

####### SERVICES MANAGEMENT

#####################

sudo systemctl disable --now nagios.service			### prevent auto start of service + stop now

############################################

#permission issue security group. user dont have permission

 
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker










