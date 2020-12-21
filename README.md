# DockerComposeX86
Docker-Compose server files for a X86 home server.

# Launch services
To launch docker-compose with many files :

- sudo docker-compose -f docker-compose.yaml -f films.yaml -f nextcloud.yaml -f seafile.yaml -f wekan.yaml -f calibre.yaml up -d --remove-orphans

# Utils

To auto restart docker containers after shutdown:

- systemctl enable --now docker.service
