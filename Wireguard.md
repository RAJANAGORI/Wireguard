- I'm assuming your are using LINUX ❤ Distro and create a directory in 
    ```
    mkdir -p /opt/WireGuard
    ```
- Provide the proper directory access to the user by using chmod and chown
- Create docker-compose.yaml file in the directory
    ```
    nano docker-compose.yaml or 
    touch docker-compose.yaml
    ```
- Now for the docker-composer content <a href="https://hub.docker.com/r/linuxserver/wireguard">linuxserver.io</a> has already made for us (I must say they are true lover of LINUX ❤ Distro)
- Now Configured the docker-compose according to your requirement
    ```
    version: "2.1"
    services:
        wireguard:
            image: ghcr.io/linuxserver/wireguard
            container_name: wireguard
            cap_add:
                - NET_ADMIN  # Perform various network related operation
                - SYS_MODULE # Load and Unload the kernal modules of linux machine
            environment:
                - PUID=1000 
                - PGID=1000
                - TZ=Asia/Kolkata # because I love India ❤
                - SERVERURL=<Private IP address of your Host machine> #optional
                - SERVERPORT=51820 #optional
                - PEERS=10 #can connect 10 client at a time
                - PEERDNS=auto #optional
                - INTERNAL_SUBNET=10.13.13.0 #This configuration provide tunnel IP to the clients
                - ALLOWEDIPS=0.0.0.0/0 #optional
            volumes:
                - /opt/WireGuard/config:/config
                - /lib/modules:/lib/modules
            ports:
                - 51820:51820/udp
            sysctls:
                - net.ipv4.conf.all.src_valid_mark=1
            restart: always
    ```