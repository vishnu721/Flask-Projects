# Solution

1. Create a project directory
```sh
mkdir project
cd project/
```

2. Check if docker and docker-compose are installed:
```sh
docker version
docker-compose version
```

3. Pull the specified docker images:
```sh
docker pull infracloudio/csvserver:latest
docker pull prom/prometheus:v2.22.0
```

4. Clone the specified repo:
```sh
git clone https://github.com/infracloudio/csvserver.git
```

5. List the pulled docker images:
```sh
docker images
```

6. Run the docker container:
```sh
docker run -d -i infracloudio/csvserver:latest
```

7. Check if the docker container is running or not.
```sh
docker ps
```

8. If the docker container is not running, check the status
```sh
docker ps -a
```

9. Check the logs and see what is the error:
```sh
docker logs 33f2890edfa6
2021/09/21 08:21:37 error while reading the file "/csvserver/inputdata": open /csvserver/inputdata: no such file or directory
```

10. Create a shell script to create the inputFile
```sh
#!/bin/bash
count=1
while [ $count -lt 11 ]
do
rand=$((RANDOM%100))
echo "$count, $rand" >> inputFile
count=$(($count+1))
done
```

11. Run the cotainer by mouting the inputfile path
```sh
docker run -d -i -v /home/ec2-user/project/csvserver/inputFile:/csvserver/inputdata infracloudio/csvserver:latest
```

12. Login to the container
```sh
docker exec -it <container-id> bash
```

13. Check the port
```sh
netstat -tulnp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp6       0      0 :::9300                 :::*                    LISTEN      1/csvserver
```

14. Stop the running container
```sh
docker stop <container-id>
```

15. Run the container and provide the port mapping
```sh
docker run -d -i -v /home/ec2-user/project/csvserver/inputFile:/csvserver/inputdata -p 9393:9300 infracloudio/csvserver:latest
```

16. Stop the container 
```sh
docker stop <container-id>
```

17. Run again by specifying the environment variable
```sh
docker run -d -i -v /home/ec2-user/project/csvserver/inputFile:/csvserver/inputdata -p 9393:9300 -e CSVSERVER_BORDER='Orange' infracloudio/csvserver:latest
```
