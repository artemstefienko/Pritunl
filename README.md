# Installing Pritunl and mongodb from a script
```
    git clone https://github.com/artemstefienko/Pritunl.git
```
>Go to directory
```
    cd Pritunl/
```
>Run script
```
    sudo bash install.sh
```
___
# Pritunl manual installation
## Update the System and tools 
```
    sudo apt update && sudo apt upgrade

    sudo apt install wget vim curl gnupg2 software-properties-common apt-transport-https ca-certificates lsb-release
```
___
## Installing Pritunl VPN Server on Ubuntu
>Add the Pritunl repository key and file to the Ubuntu system:
```
    sudo tee /etc/apt/sources.list.d/pritunl.list << EOF
    deb https://repo.pritunl.com/stable/apt jammy main
    EOF
```
>Adding it to the repository file
```
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com --recv 7568D9BB55FF9E5287D586017AE645C0CF8E292A
```
>Add the MongoDB repositories
```
    echo "deb http://security.ubuntu.com/ubuntu focal-security  main" | sudo tee /etc/apt/sources.list.d/focal-security.list
    sudo apt update
    sudo apt install libssl1.1

    wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
```
>Install Pritunl and MongoDB 
```
    sudo apt update && sudo apt install pritunl mongodb-org
```
>Start and enable the Pritunl and MongoDB
```
    sudo systemctl start pritunl mongod

    sudo systemctl enable pritunl mongod
```
___

___

___

___

___

___

___

___

___
##Proxy settings
>Connect Pritunl to MongoDB by IP address
```
    sudo nano /etc/pritunl.conf
```
>change localhost  to external ip
"mongodb_uri": "mongodb://localhost:27017/pritunl"

> Update Firewall Rules
```
    sudo apt update

    sudo apt install ufw
```
>>Allow MongoDB Ports
```
   sudo ufw allow 27017/tcp
   sudo ufw allow 28017/tcp

```
>Enable Firewall
```
    sudo ufw enable
```
___
>MongoDB config file
```
    sudo nano /etc/mongod.conf
```
>Find the bindIp line and change it to 0.0.0.0 
>>Restart MongoDB
```
    sudo systemctl restart mongod
```
___

___

___

___

___

___

___

___

___

# Install proxychains-ng
```
   sudo apt update
   sudo apt install proxychains-ng
```
>Set up proxychains-ng:
>>Edit the proxychains.conf configuration file
```
   sudo nano /etc/proxychains.conf
```
>Then add the following line to the [ProxyList] section, replacing SOCKS5_IP, SOCKS5_PORT, SOCKS5_USER, and SOCKS5_PASS with the appropriate values:
```
   socks5 SOCKS5_IP SOCKS5_PORT SOCKS5_USER SOCKS5_PASS
```
>Run Pritunl via proxychains-ng
```
 proxychains4 pritunl start
```