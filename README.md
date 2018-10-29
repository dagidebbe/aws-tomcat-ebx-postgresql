# ebx 5.9.0.1098, tomcat 9.0.12-jre11

works with EBX 5.9.0.1098, tomcat9 & jre11. see https://hub.docker.com/_/tomcat/

## create aws machine

see documentation https://docs.docker.com/machine/drivers/aws/

i created user called 'mick-docker'

required aws iam permissions, see file 'awsDockerMachine_minimal_iam_policy.txt'

```
cat ~/.aws/credentials_mick-docker
  export access_key_id=AKI...
  export secret_access_key=4mu...

source ~/.aws/credentials_mick-docker

docker-machine -D create --driver amazonec2 --amazonec2-access-key $access_key_id --amazonec2-secret-key $secret_access_key --amazonec2-open-port 9090 aws01
```

## connect to aws machine

```
docker-machine env aws01
eval $(docker-machine env aws01)
```

## Docker build

```
put your ebxLicense in ~/.profile
export EBXLICENSE=XXXXX-XXXXX-XXXXX-XXXXX
source ~/.profile
docker build -t aws-tomcat-ebx-postgresql:0.1 .
```

## Docker run

```
docker run --rm -p 9090:8080 --mount type=volume,src=ebx9,dst=/data/app/ebx -e "CATALINA_OPTS=-DebxLicense=$EBXLICENSE" --name ebx9 aws-tomcat-ebx-postgresql:0.1
docker-machine ip aws01
open http://$(docker-machine ip aws01):9090/ebx
```

## connect to running container

```
docker exec -it ebx9 /bin/bash
```

## cleanup

```
docker-machine rm aws01
```
