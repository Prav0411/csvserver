PART-1
[node1] (local) root@192.168.0.28 ~
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
[node1] (local) root@192.168.0.28 ~
$ docker run infracloudio/csvserver:latest
2023/09/02 10:40:58 error while reading the file "/csvserver/inputdata": open /csvserver/inputdata: no such file or directory
[node1] (local) root@192.168.0.28 ~
$ docker run -d --name csvserver infracloudio/csvserver:latest
a9e6710e758289c812549ffa8373e1a2d19ecebd587796e7d9068556a9e0e3f7
[node1] (local) root@192.168.0.28 ~
$ cat <<EOL > gencsv.sh
#!/bin/bash

start=\$1
end=\$2

for i in \$(seq \$start \$end); do
    echo "\$i, \$RANDOM" >> inputFile
done
EOL
[node1] (local) root@192.168.0.28 ~
$ ./gencsv.sh 2 8
bash: ./gencsv.sh: Permission denied
[node1] (local) root@192.168.0.28 ~
$ chmod +x gencsv.sh
[node1] (local) root@192.168.0.28 ~
$ ./gencsv.sh 2 8
[node1] (local) root@192.168.0.28 ~
$ docker run -d --name csvserver infracloudio/csvserver:latest
docker: Error response from daemon: Conflict. The container name "/csvserver" is already in use by container "a9e6710e758289c812549ffa8373e1a2d19ecebd587796e7d9068556a9e0e3f7". You have to remove (or rename) that container to be able to reuse that name.
See 'docker run --help'.
[node1] (local) root@192.168.0.28 ~
$ docker stop csvserver
csvserver
[node1] (local) root@192.168.0.28 ~
$ docker rm csvserver
csvserver
[node1] (local) root@192.168.0.28 ~
$ docker run -d --name csvserver -v $(pwd)/inputFile:/csvserver/inputdata -p 9393:9300 infracloudio/csvserver:latest
27db704a9c47013a43616d60a7eb86d3569947710e394109c6761fc8a51595a8
[node1] (local) root@192.168.0.28 ~
$ docker exec -it csvserver sh
sh-4.4# netstat -tuln
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp6       0      0 :::9300                 :::*                    LISTEN     
sh-4.4# exit
exit
[node1] (local) root@192.168.0.28 ~
$ docker stop csvserver
csvserver
[node1] (local) root@192.168.0.28 ~
$ docker rm csvserver
csvserver
[node1] (local) root@192.168.0.28 ~
$ docker run -d --name csvserver -v $(pwd)/inputFile:/csvserver/inputdata -p 9393:9300 -e CSVSERVER_BORDER=Orange infracloudio/csvserver:latest
8d5930a1c4aad2fffffd4fe66af8c09dfd4535765e67b2c70cf7ff297bd0e492
[node1] (local) root@192.168.0.28 ~


