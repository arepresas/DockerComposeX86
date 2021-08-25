# DockerComposeX86
Docker-Compose server files for a X86 home server.

# Launch services
To launch docker-compose with many files :

- sudo docker-compose -f docker-compose.yaml -f multimedia.yaml -f cloud-data.yaml -f office.yaml -f code.yaml -f blog.yaml -f iot.yaml up -d --remove-orphans

# Traefik

File acme.json must have 600 permissions

- chmod 600 /var/acme.json

# No-ip

Execute 
        
    "docker run --name=no-ip -d -v /etc/localtime:/etc/localtime -v ${path}:/config coppit/no-ip"

To get a blank conf file.

# Onedrive
First you have to launch the docker in interactive mode and login:

- sudo docker run -it --name onedrive -v "PATH_TO_CONF:/onedrive/conf" -v "PATH_TO_DATA:/onedrive/data" -e "ONEDRIVE_UID:UID" -e "ONEDRIVE_GID:GID" driveone/onedrive:latest

# Dropbox
Check the logs of the container to get URL to authenticate with your Dropbox account.

- docker logs dropbox

Copy and paste the URL in a browser and login to your Dropbox account to associate.

- docker logs dropbox

You should see something like this:

    "This computer is now linked to Dropbox. Welcome xxxx"

# Jobber

To add jobs, you must add a file **.jobber** in jobber folder like this example

```
[jobs]
- name: BackupServer
  cmd: backup.sh
  time: '0 0 5 * * *'
```

It will execute script **backup.sh** everyday at 5am

# Save database from container

```
docker exec CONTAINER /usr/bin/mysqldump -u root --password=root DATABASE > backup.sql
```

# Restore database to container

```
cat backup.sql | docker exec -i CONTAINER /usr/bin/mysql -u root --password=root DATABASE
```

# Utils

To auto restart docker containers after a shutdown:

- systemctl enable --now docker.service

To change Docker Root Dir:

- systemctl stop docker
- mv /var/lib/docker /destination-folder
- ln -s /destination-folder /var/lib/docker
- systemctl start docker

Fix inotify:

- sudo sysctl fs.inotify.max_user_watches=524288

Delete logs of all containers:

- sudo sh -c "truncate -s 0 /var/lib/docker/containers/*/*-json.log"

Mount SMB volumes:

- /etc/fstab

    ```
    //server-ip/ProxmoxDockerConf /ProxmoxDockerConf cifs credentials=/home/user/.smb,file_mode=0666,dir_mode=0777,nobrl,_netdev 0 0
    //server-ip/ProxmoxDockerData /ProxmoxDockerData cifs credentials=/home/user/.smb,file_mode=0666,dir_mode=0777,nobrl,_netdev 0 0
    //server-ip/ProxmoxData /ProxmoxData cifs credentials=/home/user/.smb,file_mode=0666,dir_mode=0777,nobrl,_netdev 0 0
    //server-ip/ProxmoxDivx /ProxmoxDivx cifs credentials=/home/user/.smb,file_mode=0666,dir_mode=0777,nobrl,_netdev 0 0
  
    nobrl -> To solve problem with SQLite
    _netdev -> Wait for smb mount to complete
  
    ```

- /home/user/.smb

    ```
    user=USER
    password=PASS
    domain=myDomain
    ```

# Proxmox

If you want to use this docker-compose in Proxmox is recommended to add Conf and Data folders as **Bind mount points**

https://pve.proxmox.com/wiki/Linux_Container#_bind_mount_points

- The user in host will be **GuestUser + 100000**

