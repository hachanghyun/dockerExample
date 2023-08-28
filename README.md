# Docker 

## Docker란?

    컨테이너 기반의 오픈소스 가상화 플랫폼.

    컨테이너를 관리하는 플랫폼

    다양한 이유로 바뀌는 서버, 개발 환경 문제를 해결하기 위해 등장

    기존에는 환경이 변경되면 세팅을다시 해야했는데 도커가 등장하면서 편리함 도입

## 컨테이너 기반?
   
![dockercontainer](https://github.com/hachanghyun/dockerExample/assets/33058284/c2a4b109-a4e5-4169-bcb8-a97d88d4141e)


    격리된 공간에서 프로세스가 동작하는 기술

    다양한 프로그램, 실행환경을 컨테이너로 추상화하고 동일한 인터페이스를 제공하여 프로그램의 배포 및 관리를 단순하게 해줌.

    이러한 컨테이너를 로컬pc, aws, azure, google cloud 어디에서든 실행할 수 있음

    기존의 가상머신은 OS를 가상화 시켰음. 컴퓨터의 물리적 자원을 분할하기 때문에 성능의 한계가 있었음

    도커는 OS단까지 내려가지 않고 실행환경만 독립적으로 돌림. 훨씬 빠르고 가벼움.

## 이미지??

    컨테이너 실행에 필요한 파일과 설정값등을 포함하고 있는 것.

    상태값을 가지지않고 변하지 않음.

    컨테이너는 이 이미지를 실행시킨 상태라 할 수 있음.

    같은 이미지로 여러개의 컨테이너를 만들 수 있고, 컨테이너가 삭제되도 이미지는 남아있음.

    새로운 서버를 추가할 떄, 의존성 파일들을 컴파일하고 설치할 필요 없이, 이미지들만 다운받고 컨테이너를 생성시키면 완료.

    도커허브에서 필요한 이미지들을 다운받을 수 있음.

## Dockerfile

    이미지를 만들기 위해 dokerfile이라는 파일에 이미지 생성과정을 적음.

# 도커 실치 및 실행 

## 도커 docs

도커 명령어 보는 곳

https://docs.docker.com/engine/reference/commandline/pull/

## 도커 허브

도커 이미지 다운받는 곳

https://hub.docker.com/search?type=image


## 도커 설치 

    https://docs.docker.com/desktop/windows/install/

    받은 후, 실행

!!wsl 미설치시 아래 설명에 따라 설치해주기

https://docs.microsoft.com/ko-kr/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package


완료되면 docker desktop 실행됨.
</br>
</br>
</br>
powershell을 통해 제대로 설치된 것을 확인 가능

```
PS C:\Users\nswoo> docker images

REPOSITORY          TAG          IMAGE ID       CREATED        SIZE
namusik             ubuntu-git   d53f0eefa9b4   2 months ago   206MB
namusik/python3     1.0          4465c63da8bf   2 months ago   143MB
wordpress           latest       e3a452c0a154   2 months ago   616MB
web-server-build    latest       f222fae2c5ac   2 months ago   143MB
mysql               5.7          8b43c6af2ad0   2 months ago   448MB
web-server-commit   latest       cafb6a26120a   3 months ago   143MB
httpd               latest       1132a4fc88fa   3 months ago   143MB
ubuntu              20.04        ba6acccedd29   4 months ago   72.8MB
ubuntu              latest       ba6acccedd29   4 months ago   72.8MB
```

## 이미지 pull

docker hub에서 이미지를 다운받는 것을 pull이라 함.

https://hub.docker.com/search?type=image

원하는 컨테이너들 검색한다.

![dockerhub](https://github.com/hachanghyun/dockerExample/assets/33058284/fa0685a8-7f85-4bd9-920a-732389525fa9)


우측 명령어를 복사해서 컨테이너의 이미지를 pull 할 수 있음.

```
docker pull redis
```

다운을 확인하려면 도커 데스크탑의 images 탭을 클릭하거나 

docker images 명령어를 실행시켜본다.

</br>
</br>

## 이미지 Run 

#### 도커 데스크탑 

    1. images 우측에 run 클릭
    2. container 이름 설정해주기
    3. container가 생성되고 실행되는 중으로 변함.
    4. 멈추고싶으면 stop 하면 됨

#### 명령어
```
    <이미지 Run>
    docker run [OPTIONS] IMAGE [COMMAND]

    ex) docker run --name redisTest redis
```
```
    <실행중인 container 확인>
    docker ps
```
```
    <컨테이너 stop>
    docker stop [OPTIONS] CONTAINER [CONTAINER...]

    ex) docker stop redisTest
```
```
    <전체 container 확안>
    docker ps -a
```
```
    <container 다시 실행시키기>
    docker start [OPTIONS] CONTAINER [CONTAINER...]
    
    ex) docker start redisTest
```
```
    <container 삭제하기>
    docker rm [OPTIONS] CONTAINER [CONTAINER...]

    먼저 중지 시키고 삭제해야함

    ex) docker rm redisTest
```
``` 
    <이미지 삭제하기>
    docker rmi [OPTIONS] IMAGE [IMAGE...]

    docker rmi redis
```

## 도커로 웹서버 구성
</br>

![dockerhost](https://github.com/hachanghyun/dockerExample/assets/33058284/c9942773-3b21-4593-a597-4c03767b14f3)


#### 과정 

    1. 웹서버가 container에 설치됨.
    2. 이 Container가 설치된 운영체제를 Docker Host라 부름. 
    3. container와 host 모두 독립적인 port와 file system을 가지고 있음
    4. 웹 브라우저에서 접속을 하려면 
    5. host의 80번과 container의 80번 port를 연결시켜야 함
    6. docker run -p 80(host의 포트):80(container의 포트) httpd
    7. host의 80번으로 들어온 요청이 container의 80번 포트로 이동함.
    8. 이것을 포트포워딩이라 함.

#### 포트 번호 지정한 container

도커 데스크탑

    1. local host : host의 포트번호
    2. container port : 이미지가 설치될 container의 포트번호

명령어

    docker run --name ws1 -p 8080:80 httpd

브라우저에서 localhost:8080 접속하면 container 안에 있는 index.html을 읽어드림.

## Container 안에 있는 파일 수정하기 

docker desktop 

1. container 클릭 후 우측 상단에 cli 클릭
2. container 안에서 명령을 실행 시킬 수 있게 됨

powershell 

```
    docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

    ex) docker exec -it ws1 /bin/bash 혹은 /bin/sh

    -it : interactive, tty 조합해서 만든 옵션. 컨테이너와 지속적으로 연결을 유지할 때

    shell 프로그램 실행. 사용자가 입력한 명령을 받아서 OS에 전달해주는 일종의 창구

    <container와 연결 종료>
    exit
```
```
    httpd의 index.html이 있는 위치로 이동.

    apt update
    apt install nano
    nano index.html 
    수정하고 컨트롤 O. 나갈떄는 컨트롤 X
```

## Host의 파일을 Container가 반영할 수 있도록 

    docker run -p 8082:80 -v C:\Users\nswoo\Desktop\docker\htdocs\:/usr/local/apache2/htdocs/ httpd
   
# Docker에 Mysql 설치하기 

## 명령어 

docker run --platform linux/amd64 
-p 3306:3306 
--name [컨테이너 이름] 
-e MYSQL_ROOT_PASSWORD=[루트 유저 비밀번호] 
-e MYSQL_DATABASE=[데이터베이스 이름]
-e MYSQL_USER=[유저 이름]
-e MYSQL_PASSWORD=[비밀번호] 
-d mysql

    <이미지 다운>
    docker pull mysql

    <이미지 run>
    docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=password --name mysqlCont mysql

혹은 비밀번호를 지정안하려면

    docker run -d -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=yes --name mysqlCont mysql

    docker run --platform linux/amd64 -p 3306:3306 --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=YES -e MYSQL_DATABASE=SALESMEMO_LOCAL -d mysql:5.6

    //ONLY_FULL_GROUP_BY 에러제거
    --sql-mode="STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION"

    <mysql 컨테이너 접속>
    docker exec -it mysqlCont /bin/bash

    <비밀번호 입력>
    mysql -u root -p

    비밀번호 입력창 나옴

    <비밀번호 없으면>
    mysql -u root

AWS RDS Mysql 접속 명령어

~~~sh
mysql -h 엔드포인트 -P 포트번호 -u 유저네임 -p
~~~

## 미사용중인 docker images 삭제하기
~~~sh
docker images --format '{{.Repository}}:{{.Tag}}' | grep -vFf <(docker ps -a --format '{{.Image}}' | sort -u)
~~~

This command works by:

1. Running the **docker images** command to list all Docker images.
2. Using the **--format** option to specify the format of the output to be just the repository and tag of each image in "repository:tag" format.
3. Piping the output of **docker images** to **grep**.
4. Using the **-v** option to invert the match and show only lines that do not match.
5. Using the **-F** and **-f** options to search for patterns from a file.
6. Using process substitution **<(...)** to pass the output of **docker ps -a --format '{{.Image}}' | sort -u** as a file to **grep**.
7. **docker ps -a --format '{{.Image}}'** lists the IDs of all images used by all containers, both running and stopped. The **--format** option specifies the format of the output. **{{.Image}}** extracts the ID of the image used by the container.
8. **sort -u** sorts the list of used image IDs and removes duplicates.

This will output a list of Docker image names in the "repository:tag" format that are not in use by any running or stopped containers.
