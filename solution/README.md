###############################################################
#                          WARNING!!!!                        #
# This is a sandbox environment. Using personal credentials   #
# is HIGHLY! discouraged. Any consequences of doing so are    #
# completely the user's responsibilites.                      #
#                                                             #
# The PWD team.                                               #
###############################################################
[node1] (local) root@192.168.0.8 ~
$ docker pull infracloudio/csvserver:latest
docker pull prom/prometheus:v2.22.0
latest: Pulling from infracloudio/csvserver
ae43b40a9945: Pull complete 
7bb33bb2db38: Pull complete 
c82d72e1bb76: Pull complete 
Digest: sha256:20bc5a93fac217270fe5c88d639d82c6ecb18fc908283e046d9a3917a840ec1f
Status: Downloaded newer image for infracloudio/csvserver:latest
docker.io/infracloudio/csvserver:latest
v2.22.0: Pulling from prom/prometheus
76df9210b28c: Pull complete 
559be8e06c14: Pull complete 
97170ed2e56a: Pull complete 
4a6c0b5646ca: Pull complete 
f6776fcc9f18: Pull complete 
7eed139cfec6: Pull complete 
c0c3c15c8e94: Pull complete ad6e678f5b25: Pull complete 
9a8236411762: Pull complete 
0cfb39b876cc: Pull complete 
ffe345581c7a: Pull complete 
033c1a7f7349: Pull complete 
Digest: sha256:60190123eb28250f9e013df55b7d58e04e476011911219f5cedac3c73a8b74e6
Status: Downloaded newer image for prom/prometheus:v2.22.0
docker.io/prom/prometheus:v2.22.0
[node1] (local) root@192.168.0.8 ~
$ docker run -d --name csvserver infracloudio/csvserver:latest
af54732cb930bff764eb18529eb68af876f619be4ac7fad250d305d946de2bd6
[node1] (local) root@192.168.0.8 ~
$ if [ "$(docker inspect -f '{{.State.Status}}' csvserver)" != "running" ]; then
    echo "Container failed to start."
    exit 1
fi
Container failed to start.
logout
###############################################################
#                          WARNING!!!!                        #
# This is a sandbox environment. Using personal credentials   #
# is HIGHLY! discouraged. Any consequences of doing so are    #
# completely the user's responsibilites.                      #
#                                                             #
# The PWD team.                                               #
###############################################################
[node1] (local) root@192.168.0.8 ~
$ cat <<EOL > gencsv.sh
#!/bin/bash

start=\$1
end=\$2

for i in \$(seq \$start \$end); do
    echo "\$i, \$RANDOM" >> inputFile
done
EOL
[node1] (local) root@192.168.0.8 ~
$ chmod +x gencsv.sh
[node1] (local) root@192.168.0.8 ~
$ ./gencsv.sh 2 8
[node1] (local) root@192.168.0.8 ~
$ docker stop csvserver
csvserver
[node1] (local) root@192.168.0.8 ~
$ docker rm csvserver
csvserver
[node1] (local) root@192.168.0.8 ~
$ docker run -d --name csvserver -v $(pwd)/inputFile:/csvserver/inputdata
"docker run" requires at least 1 argument.
See 'docker run --help'.

Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Create and run a new container from an image
[node1] (local) root@192.168.0.8 ~
$ docker run -d --name csvserver
"docker run" requires at least 1 argument.
See 'docker run --help'.

Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Create and run a new container from an image
[node1] (local) root@192.168.0.8 ~
$ docker run -d --name csvserver
"docker run" requires at least 1 argument.
See 'docker run --help'.

Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Create and run a new container from an image
[node1] (local) root@192.168.0.8 ~
$ docker run -d --name csvserver infracloudio/csvserver:latest
a7bc82c2cf36bd6b5a860d2b3770162e578b38043267a025ccd0a79a6b7bbe1b
[node1] (local) root@192.168.0.8 ~
$ ps
PID   USER     TIME  COMMAND
    1 root      0:00 /bin/sh -c cat /etc/hosts >/etc/hosts.bak &&     sed 's/^::1.*//' /etc/hosts.bak > /etc/hosts &&   
   16 root      0:20 dockerd
   38 root      0:00 sshd: /usr/sbin/sshd -o PermitRootLogin=yes -o PrintMotd=no [listener] 0 of 10-100 startups
   75 root      0:03 containerd --config /var/run/docker/containerd/containerd.toml --log-level debug
 1029 root      0:00 script -q -c /bin/bash -l /dev/null
 1030 root      0:00 /bin/bash -l
 2946 root      0:00 ps
[node1] (local) root@192.168.0.8 ~
$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[node1] (local) root@192.168.0.8 ~
$ version: '1'
services:
  csv-generator:
    image: infracloudio/csvserver:latest  # Replace with the name and tag of your base image
    environment:
      - CSVSERVER_BORDER=Orange  # Replace with your desired environment variable and value
    ports:
      - "0.0.0.0:9393:9300"  # Map host port 8080 to container port 80
    volumes:
      - ./inputdata:/csvserver/inputdata  # Map a volume for the output file
    networks:
      - my-network

  Prometheus:
    image: prom/prometheus:v2.22.0
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"
    networks:
      - my-network
networks:
  my-network:
    driver: bridge
bash: version:: command not found
bash: services:: command not found
bash: csv-generator:: command not found
bash: image:: command not found
bash: environment:: command not found
bash: -: command not found
bash: ports:: command not found
bash: -: command not found
bash: volumes:: command not found
bash: -: command not found
bash: networks:: command not found
bash: -: command not found
bash: Prometheus:: command not found
bash: image:: command not found
bash: volumes:: command not found
bash: -: command not found
bash: ports:: command not found
bash: -: command not found
bash: networks:: command not found
bash: -: command not found
bash: networks:: command not found
bash: my-network:: command not found
bash: driver:: command not found
[node1] (local) root@192.168.0.8 ~
$ docker run -d --name csvserver -v $(pwd)/inputFile:/csvserver/inputdata -p 9393:9300 infracloudio/csvserver:latest
docker: Error response from daemon: Conflict. The container name "/csvserver" is already in use by container "a7bc82c2cf36bd6b5a860d2b3770162e578b38043267a025ccd0a79a6b7bbe1b". You have to remove (or rename) that container to be able to reuse that name.
See 'docker run --help'.
[node1] (local) root@192.168.0.8 ~
$ docker rm csvserver
csvserver
[node1] (local) root@192.168.0.8 ~
$ docker run -d --name csvserver -v $(pwd)/inputFile:/csvserver/inputdata -p 9393:9300 infracloudio/csvserver:latest
724c9f513e463827a97dbcf5f14887c6542df1b0d944c9e195a213b261b77abb
[node1] (local) root@192.168.0.8 ~
$ docker exec -it csvserver sh
sh-4.4# netstat -tuln | grep 9300
tcp6       0      0 :::9300                 :::*                    LISTEN     
sh-4.4# exit
exit
[node1] (local) root@192.168.0.8 ~
$ docker stop csvserver
csvserver
[node1] (local) root@192.168.0.8 ~
$ docker rm csvserver
csvserver
[node1] (local) root@192.168.0.8 ~
$ docker run -d --name csvserver -v $(pwd)/inputFile:/csvserver/inputdata -p 9393:9300 -e CSVSERVER_BORDER=Orange infracloudio/csvserver:latest
4e16c20cf54bf0e840e7216b94b96675ec039ef86e44a859528aeea57ab30ee7
[node1] (local) root@192.168.0.8 ~
$ 
