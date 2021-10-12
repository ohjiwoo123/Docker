# Docker-Basic

## Docker 기본 사용법 정리 

- 레퍼런스 및 도커 관련 가이드 
https://docs.docker.com

- image 다운로드 
https://hub.docker.com

## Docker Command 

### Docker Basic Command
- 새로운 컨테이너를 실행  
`docker run httpd` 

- 실행중인 컨테이너를 보여줌  
`docker ps`  


- (중지 된 컨테이너도 보여줌)   
`docker ps -a` 

- ws2라는 이름의 아파치 컨테이너 실행  
`docker run —name ws2 httpd`

- 아파치 서버 종료  
`docker stop httpd`

- ws2 컨테이너 재실행   
`docker start ws2`

- ws2 컨테이너 로그 보기  
`docker logs ws2`

- 컨테이너 삭제  
`docker rm ws2`

- 이미지 삭제  
`docker rmi httpd` 

Docker Host - 호스트가 여러 컨테이너를 포함한다. 독립적인 포트를 가진다.  
Docker Container - 호스트 안에 여러 컨테이너가 있다. 독립적인 포트를 가진다.

- 앞에 80 = host , 뒤에 80 = container  
`docker run -p 80:80 httpd`			

- 컨테이너의 CLI를 입력할 때 == exec  
`docker exec ws3 pwd`  
출력 값 == /usr/local/apache2  

- 컨테이너와 지속적으로 연결하고 싶을 때  
`docker exec -it ws3 /bin/sh`

- 컨테이너의 파일시스템과 호스트의 파일 시스템을 연결  
`docker run -p 8888:80 -v ~/Desktop/htdocs:/usr/local/apache2/htdocs/ httpd`

============================================   
### Dockerfile & build

![스크린샷 2021-10-07 오전 10 44 40](https://user-images.githubusercontent.com/80387186/136656651-b203cd75-db8d-4d82-9bf7-fe8e93205fc2.png)
Container를 commit하여 image를 만들 수 있다.
Dockerfile을 통해서 build하여 이미지를 만들 수 있다. 

```
# (Dockerfile이 있는 폴더에서 . 사용하면 됨.) -t (tag이름) 
docker build -t web-server-build .; 
docker rm --force web-server; 
docker run -p 8888:8000 --name web-server web-server-build pwd;
docker commit web-server web-server-commit 
```

====================================================

### Docker Commit 연습

`docker pull ubuntu`  
`docker run -it --name my-ubuntu ubuntu bash`  
`docker commit my-ubuntu jwoh:ubuntu-git`  

====================================================

### Docker file 예시
```
FROM ubuntu:20.04
RUN apt update && apt install -y python3 
WORKDIR /var/www/html
COPY ["index.html", "."]
EXPOSE 8000
ENTRYPOINT ["python3", "-u", "-m", "http.server"]
```

====================================================

### Docker Push 연습 
먼저 hub.docker.com에 로그인해서 repository를 새로 만들어야한다.  
`docker login`  
`docker run -it --name my-python ubuntu`  
`docker commit my-python tebin/python3:1.0`  
(hub.docker.com)에서 레파지토리를 만들어야한다.  
`docker push tebin/python3:1.0`  
(무료로는 단 1개의 repo만 사용 가능하다.)  

====================================================
### Docker Hub Repository 말고 github 패키지 이용하기

*참고 github 사이트가서 package로 들어가고 settings -> developer settings에서 개인 토큰 값 받아야도 됨  

Docker Github.com Packages Container Registry  

`docker login ghcr.io -u tebin -p TOKEN`  
== (참고로 토큰은 깃허브에서 받는 개인 토큰 값)  
`export CR_PAT=TOKEN`  
`echo $CR_PAT TOKEN`  
`echo $CR_PAT | docker login ghcr.io -u USERNAME —password-stdin`  

====================================================

### Reference 
- 생활코딩 도커편
https://www.youtube.com/channel/UCvc8kv-i5fvFTJBFAk6n1SA
