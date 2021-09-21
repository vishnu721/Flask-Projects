# Solution

1. Create a project directory
```sh
mkdir project
cd project/
```

Check if docker and docker-compose are installed:
```sh
docker version
docker-compose version
```

Pull the specified docker images:
```sh
    docker pull infracloudio/csvserver:latest
    docker pull prom/prometheus:v2.22.0
```

Clone the specified repo:

git clone https://github.com/infracloudio/csvserver.git

List the pulled docker images:
docker images

Run the docker container:

docker run -d -i infracloudio/csvserver:latest

Check if the docker container is running or not.
docker ps

If the docker container is not running, check the status
docker ps -a

Check the logs and see what is the error:
docker logs 33f2890edfa6
2021/09/21 08:21:37 error while reading the file "/csvserver/inputdata": open /csvserver/inputdata: no such file or directory

Create a shell script to create the inputFile
#!/bin/bash
count=1
while [ $count -lt 11 ]
do
rand=$((RANDOM%100))
echo "$count, $rand" >> inputFile
count=$(($count+1))
done

Run the cotainer by mouting the inputfile path
docker run -d -i -v /home/ec2-user/project/csvserver/inputFile:/csvserver/inputdata infracloudio/csvserver:latest

Login to the container
docker exec -it 7b9b447264f5 bash

Check the port
netstat -tulnp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp6       0      0 :::9300                 :::*                    LISTEN      1/csvserver

Stop the running container
docker stop 7b9b447264f5

Run the container and provide the port mapping
docker run -d -i -v /home/ec2-user/project/csvserver/inputFile:/csvserver/inputdata -p 9393:9300 infracloudio/csvserver:latest

Stop the container 
docker stop 8ef54e4ee3d3

Run again by specifying the environment variable
docker run -d -i -v /home/ec2-user/project/csvserver/inputFile:/csvserver/inputdata -p 9393:9300 -e CSVSERVER_BORDER='Orange' infracloudio/csvserver:latest
