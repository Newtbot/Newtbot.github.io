---
author: "Ti Seng"
title: "Setting up a simple paper minecraft server"
date: "2026-08-26"
description: "Configuring a simple minecraft server on a cointainer / vps with paper.jar for developmnet or simply self hosting a game server for your friends. Without using pterodactyl or crafty and just a simple lightweight container."
tags: ["linux", "minecraft", "container" , "promox"]
categories: ["system administration , promox" ]
# series: [""]
# cover:
#   image: img/hugo.png
ShowToc: true
TocOpen: false
weight: 1

---


<!-- {{< youtube hjD9jTi_DQ4 >}} -->
# Before we start
Before we start i am going with some assumptions, you have some prior linux experience and know how to configure a simple VPS or a cointainer. For my usecase, i will be using a promox LCX on my own homelab. 

Refer to this Youtube video on what the hell is [promox](https://www.youtube.com/watch?v=f-kvuLoFTWQ)

## Making the container

Skip this portion if you are using a VPS...

#### 1. Click on "Create CT".
[![Promox Dashboard](/img/mc-server/promox.png)](/img/mc-server/promox.png)

#### 2. LCX creation Wizard.
[![Promox Dashboard](/img/mc-server/lcx-wizard.png)](/img/mc-server/lcx-wizard.png)

- Choose the relevant "Node"
- Your own "ID" of your choice
- hostname of your choice
- password / SSH keys 

#### 3. The Rest of the configration difers from  each "Promox Node". My own Node has its own Dir attached with ISOs and networking portion. (Do read up on how to configure LCX and Promox)

## Post Cointainer Creation 

#### 1. LCX Container created. 
[![Cotanine and VMs](/img/mc-server/container.png)](/img/mc-server/cointainer.png)

#### 2. You would either SSH key or keyless and proceed with the setup. 
- SSH user@<hostname/Host IP> 

#### 3. Setting up the cointaner with the necessary tools. 
- If you are using a iso image usually they come lean and light... you would actually need to install your own tools like vim or vi 
- If you have a ansible playbook to use... would be nice to use.

**IMPORTANT THING TO DO:**
**REMEMBER TO RUN APT-GET UPGRADE & APT-GET UPGRADE -Y**

## MC server Jar Portion (Jump here if you are using a VPS)

#### Links
- The PaperMC documentation [PaperMC.io](https://docs.papermc.io/paper/getting-started/)
- Download Page
(https://papermc.io/downloads/paper)

#### 1. Installing the Paper.jar and setting up the MC server. 
- In the Terminal run "wget https://fill-data.papermc.io/v1/objects/a8c9140c3075bd7c04973e9cdc491b21bfe6bad472b674ef932a4ae0fec19629/paper-26.2-119.jar"

- To Start the Server. 
  - Either Run the command where the file is "java -Xms4G -Xmx4G -jar <paper-version-name.jar> --nogui" 
  - Or with a Start-up script 
  (https://docs.papermc.io/misc/tools/start-script-gen/)

- Startup Script Portion...
1. create the server.sh by running the command "touch server.sh"
2. paste the script generated from (https://docs.papermc.io/misc/tools/start-script-gen/)
3. The script should look like this..
4. Note the location.. I chose to create a new home directory for the mc-server
5. create a systemd file 
6. enable the systemd file and start it.

**server.sh**
```sh
root@mc-server:/home/noot# cat /home/minecraft/mc-server/server.sh
#!/usr/bin/env sh

java -Xms1024M -Xmx1024M -jar paper-1.21.11-132.jar --nogui
root@mc-server:/home/noot# cat /home/minecraft/mc-server/server.sh
#!/usr/bin/env sh

java -Xms1024M -Xmx1024M -jar paper-1.21.11-132.jar --nogui
```

**systemd file**
```
[Unit]
Description=Minecraft Paper Server
After=network.target

[Service]
WorkingDirectory=/home/minecraft/mc-server
User=noot
Group=noot
ExecStart=/home/minecraft/mc-server/server.sh
Restart=on-failure
RestartSec=10
StandardInput=null

# Give it time to save the world on shutdown
TimeoutStopSec=60
KillSignal=SIGTERM

[Install]
WantedBy=multi-user.target
```

This is a random guide and writeup done by me and for me to remember and document what i have learned. **THIS IS NOT A PERFECT WORLD GUIDE WITH BEST PRACTICE. I MADE THIS TO TEST MY MINECRAFT BOT IN MIND**