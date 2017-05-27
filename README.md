Welcome to All in one Docker seedbox !
===================


I decided to experiment docker and docker-compose and I decided to assembly a lot of tools for create a seedbox environment.
You don't need to install no other package on your system that docker. Amazing no ?

*All tools are exposed with https*

----------


Tools embedded
-------------

- Transmission : trans.yourdomain.com
- ruTorrent : dl.yourdomain.com
- Jackett (Torrent search) : jackett.yourdomain.com
- Sonarr : sonarr.yourdomain.com
- Radarr : radarr.yourdomain.com
- Plex server

Installation and configuration
-------------

### Setup your seedbox
- Install docker on your system
- Install docker-compose
- Create a user

 ```
sudo useradd -m -d /home/seedbox seedbox
 ```
- add user in docker group

 ```
sudo usermod -aG docker seedbox
 ```

- Now clone this repository in the user home space
-- git clone ...

### setup environment

Copy the file .env.template in .env
Modify the file
You can bought a subdomain for only 2€/year on OVH and set all the subdomain necessary. This is mandatory because all tools are exposed only on the https port and use validate certificate.
If you don't want to spend 2€ you can set all your domain in your host file ... Ask to google.

###Start all tools

In the root folder execute
```
docker-compose up -d
```

### Setup security

Refer to this documention https://github.com/jwilder/nginx-proxy#basic-authentication-support
You need to create for each {subdomain}.{domain} a file with the same template in folder *./config/nginx-proxy-htpasswd/*
You can use this tool if you want no install htpasswd on your system for generate a pair login/password
```
docker run --rm -ti xmartlabs/htpasswd user password > ./config/nginx-proxy-htpasswd/passwordFile
```
If you want to define the same password for all tools, you can used only one file containing password
```
cd ./config/nginx-proxy-htpasswd/
ln -s passwordFile dl.mysubdomain.com
ln -s passwordFile trans.mysubdomain.com 
ln -s passwordFile sonarr.mysubdomain.com 
ln -s passwordFile radarr.mysubdomain.com
ln -s passwordFile jackett.mysubdomain.com 
```

### Configure Plex

Go on http://yourdomain.com:32400

### Configure Jackett
Go on https://jackett.yourdomain.com and register all your torrent board as you want.
This tool exposed a standard for all tools to search contents.

### Configure Sonarr & Radarr
Go on https://sonarr.yourdomain.com

 **Set Download client - Transmission**
 * Name : transmission
 * Host: transmission
 * port: 9091 
 **Set Download client - ruTorrent**
 * Name : rTorrent
 * Host: rtorrent
 * port: 80
 * Path: /plugins/rpc/rpc.php
 **Set Indexer - torznab custom (example, used your own jackett infornation)**
 * Name: The Pirate Bay
 * Host: http://jackett:9117/torznab/thepiratebay
 * Api key: Jackett api key

>**Proposal**
>Don't hesitate to improve and share your proposal. The Pull request buton is on the top of this site ;)

Have fun !


----------


> **Disclaimer:**

> Informations contained on this respository is for general information purposes only. You should not rely upon the material or information on the repository as a basis for making any business, legal or any other decisions. I make no representations or warranties of any kind, express or implied about the completeness, accuracy, reliability, suitability or availability with respect to the solution contained on the repository for any purpose. Any reliance you place on such material is therefore strictly at your own risk.
