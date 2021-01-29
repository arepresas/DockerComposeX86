# DockerComposeX86
Docker-Compose server files for a X86 home server.

# Launch services
To launch docker-compose with many files :

- sudo docker-compose -f docker-compose.yaml -f multimedia.yaml -f cloud-data.yaml -f office.yaml -f code.yaml -f blog.yaml -f wordpress.yaml up -d --remove-orphans

# Utils

To auto restart docker containers after shutdown:

- systemctl enable --now docker.service
