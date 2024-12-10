## Dockefile이란?
Docker Image를 build하기 위한 파일이다.
- 이렇게 만든 Docker Image(앱을 실행하기 위한 모든 것을 담고 있는 것)를 실행하면 Docker Container가 된다. 즉 앱이 필요로 하는 모든 것을 한 곳에 담을 수 있다.
- 마치 요리 레시피와 같이, 누구나 Dockerfile을 보고 똑같은 앱 환경을 만들 수 있다.
- Dockerfile을 작성 시, 앱을 만드는 과정을 자동화할 수 있다. 매번 같은 방식으로 앱을 만들고 배포할 수 있기에, 즉 **앱을 만드는 일이 더욱 쉬워지고, 누구나 똑같은 결과를 얻을 수 있게된다.**

----

### Dockerfile 명령어

- **FROM**: 베이스 이미지를 선택
    - ex) `FROM ubuntu:22.04`

- **MAINTAINER**: Dockerfile을 작성한 사람의 정보를 입력
    - ex) `MAINTAINER naebaecaem <nbcamp@spartacoding.co>`
    - 이미지의 작성자 또는 유지보수자의 이름과 이메일 주소를 Dockerfile에 명시할 수 있는 명령어이다. 이미지의 소유자나 개발자 정보를 추적하는 데 도움이 되며, 해당 이미지에 대한 지원이나 문의를 받기 위해 사용된다. 
    - 단, 현재 Docker(Docker 1.13부터)는 **MAINTAINER 명령어를 더 이상 권장하지 않고**, 대신, `LABEL`명령어를 사용하여 메타데이터를 추가하는 방식으로 대체되었다.

- **LABEL**: 이미지에 메타데이터를 추가
    - ex) `LABEL purpose='nginx test'`
    - **메타데이터(Metadata)**란, 데이터를 설명하거나 그에 대한 정보를 제공하는 데이터다. 
    쉽게 말해, 메타데이터는 "데이터에 대한 데이터"이다. Docker에서는 이미지와 컨테이너에 대한 추가 정보를 기록하기 위해 메타데이터를 사용한다. 이 메타데이터는 이미지를 설명하거나 관리하는 데 유용하다.  
        - 이외 Docker 메타데이터:
            - `maintainer`: 이미지의 작성자나 유지보수자의 이름과 연락처 정보.
            - `version`: 이미지의 버전.
            - `description`: 이미지의 설명.
            - `license`: 이미지가 사용하는 라이센스 정보. 

- **RUN**: 이미지를 생성하는 동안 실행할 명령어를 입력, 이미지에 명령을 실행하여 파일을 추가하거나 삭제
    - 사용자를 지정하지 않은 상태라면, root 로 실행
    - ex) `RUN apt update && apt upgrade -y && apt autoremove && apt autoclean`
    - ex) `RUN apt install openjdk-21-jdk`

- **CMD**: Command, 컨테이너를 생성할 때, 실행할 명령어를 입력
    - 컨테이너를 생성할 때만 실행
    - 추가적인 명령어에 따라 설정한 해당 명령어 수정 가능
    - ex) `CMD ["nginx", "-g", "daemon off;"]`

- **ENTRYPOINT**: 컨테이너 시작할 때, 실행할 명령어를 입력
    - 컨테이너를 시작할 때마다 실행
    - 추가적인 명령어 존재 여부와 상관 없이 무조건 실행
    - **컨테이너 생성이 아닌 시작**인 점 주의
    - ex) `ENTRYPOINT ["npm", "start"]`


- **ENV**: 환경 변수를 설정
    - 이미지 안에 각종 환경 변수를 지정
    - ex) `ENV STAGE staging`
    - ex) `ENV JAVA_HOME /usr/lib/jvm/java-8-oracle`

- **WORKDIR**: 작업 디렉토리(=위킹 디렉토리=Working directory)를 지정
    - ex) `WORKDIR /app`
    - 디렉토리가 없으면 만들어서 지정
    
- **COPY**: 파일을 이미지에 복사
    - 호스트의 파일이나 디렉토리를 이미지 안에 복사
    - Docker Context, 즉, 빌드 작업 디렉토리 내 파일만 복사 가능
    - ex) `COPY index.html /usr/share/nginx/html`



- **USER**: 사용자를 설정
    - Container의 기본 사용자는 root이며, root 권한이 필요 없는 application이라면 다른 사용자로 변경하여 사용해야한다.
```
RUN ["useradd", "user"]
USER user
RUN ["/bin/bash, "-c", "ls"]
```

- **EXPOSE**: 컨테이너에서 노출할 포트를 지정
    - ex) `EXPOSE 80`
    - ex) `EXPOSE 443`


----

### 원리: Dockerfile-> Docker Image -> Docker Container 흐름의 원리

1. Image를 생성하려면 Docker CLI를 사용하여 다음과 같은 명령을 실행한다.
    
```
    docker build -t my-nginx:latest . 
```
- 위 명령어는 현재 디렉토리에서 Dockerfile을 기반으로 my-nginx:latest라는 이름의 Docker Image를 생성하는 예제이다. (my-nginx라는 이름의 Image에 latest 태그를 붙여서 build)
- 위 명령이 실행되지 않는 경우 `docker buildx build -t my-nginx:latest .`를 추가로 실행해주면 된다.   
    - `buildx` : Docker의 Image build 기능을 확장하는 도구이다. Docker BuildKit을 기반으로 하며, 여러 기능을 제공한다.
- 참고로 `-t`는 tag를 의미, `.`은 현재 디렉토리를 의미한다.


2. 이렇게 생성된 Docker Image는 Docker CLI를 사용하여 컨테이너로 실행할 수 있다. 예를 들어, 다음과 같은 명령어를 사용하여 컨테이너를 실행할 수 있다.

```
docker run -d -p 80:80 my-nginx:latest

```
- 위 명령어는 my-nginx:latest Image를 기반으로 컨테이너를 실행하고, 80번 포트(앞)를 호스트 머신의 80번 포트(뒤)로 맵핑(maping)하는 예제이다.


3. 이렇게 생성된 컨테이너는 Docker CLI를 사용하여 종료하거나 업데이트할 수 있다. 예를 들어, 다음과 같은 명령어를 사용하여 컨테이너를 종료할 수 있다.

```
docker stop my-nginx
```
- 위 명령어는 my-nginx라는 이름의 컨테이너를 종료하는 예제이다.
- 꼭! **stop**를 해줘야한다. **그냥 창을 닫았을 경우 종료되지 않는다는 점**을 주의하자.

4. 이렇게 생성된 Docker 이미지는 Docker 레지스트리를 사용하여 다른 사용자와 공유할 수 있다. Docker 레지스트리를 사용하면 Docker 이미지를 저장하고 공유할 수 있고, 다른 사용자가 이미지를 다운로드하여 사용할 수 있다.
- Docker Image를 만들고, 내 로컬에서 컨테이너를 실행하기도 하지만, 레지스트리(Registry)라는 곳에 저장되기도 한다. Docker 레지스트리는 지금까지 알게 모르게 계속 사용되고 있는 Docker Hub 레지스트리, 또는 AWS와 같은 AWS 컨테이너 레지스트리, Azure, Google 등 이런 곳을 사용할 수 있다.
 
----

**Dockerfile 명령어를 사용한 Dockerfile 예제1**
이는 Dockerfile은 Ubuntu 최신 버전을 기반으로 Nginx를 설치하고, index.html 파일을 Nginx의 HTML 디렉토리에 복사하는 예시이다.
```
FROM ubuntu:latest
MAINTAINER Your Name <your-email@example.com>
RUN apt-get update && apt-get install -y nginx
COPY index.html /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
--- 
**Dockerfile 명령어를 사용한 Dockerfile 예제2**
```
# Python 3.11 기반 이미지
# Python 3.11이 설치된 Linux 기반의 환경을 포함하고 있음
FROM python:3.11

# pipenv 설치
# pipenv는 Python 프로젝트의 종속성을 관리하는 도구
RUN pip install pipenv

# 작업 디렉터리 설정
# /app 디렉터리로 작업 디렉터리를 설정. 이후의 명령들은 모두 이 디렉터리에서 실행
WORKDIR /app

# 현재 디렉터리(로컬 컴퓨터)의 모든 파일을 Docker 이미지의 /app 디렉터리로 복사
# 이는 프로젝트 파일을 Docker 컨테이너에 포함시키는 단계
ADD . /app/

# pipenv로 Python 3.11 환경에서 가상 환경 생성
# pipfile에 정의된 종속성을 설치
RUN pipenv --python 3.11

# pipenv 환경 안에서 poetry를 설치
# poetry는 또 다른 Python 패키지 관리 도구로, 프로젝트 관리 및 종속성 관리를 도와줌
RUN pipenv run pip install poetry

# pipenv로 종속성 설치
# pipenv sync는 Pipfile.lock에 정의된 정확한 종속성 버전을 설치(종속성들 맞춰줌)
RUN pipenv sync

# pipenv 환경 내에서 certifi 패키지를 설치
# certifi는 SSL 인증서 번들을 제공하는 Python 패키지
RUN pipenv run pip install certifi

# 빌드 인자 STAGE 정의
# Docker 빌드 시 지정될 수 있다
ARG STAGE

# .env 파일에 환경 변수 추가
# PYTHONPATH=.는 현재 디렉터리를 Python 모듈 경로에 추가하는 설정
RUN sh -c 'echo "STAGE=$STAGE" > .env'
RUN sh -c 'echo "PYTHONPATH=." >> .env'

# 스크립트 실행 권한 설정
# ./scripts/run.sh와 ./scripts/run-worker.sh 스크립트 파일에 실행 권한을 부여
RUN chmod +x ./scripts/run.sh
RUN chmod +x ./scripts/run-worker.sh

# 컨테이너 시작 시 기본적 실행할 명령어 설정
# 컨테이너가 시작될 때 ./scripts/run.sh 스크립트를 실행하도록 지정
CMD ["./scripts/run.sh"]
```

정리 : 위 Dockerfile은 Python 3.11 환경을 설정하고, pipenv와 poetry를 사용하여 종속성을 관리한다. 그 후 필요한 패키지를 설치하고, .env 파일을 생성하여 환경 변수를 설정하며, 실행할 스크립트에 실행 권한을 부여한다. 마지막으로, 컨테이너가 실행될 때 run.sh 스크립트를 실행하도록 설정한다.

----
**Dockerfile 명령어를 사용한 Dockerfile 예제3**
- Nginx 이미지를 생성하는 Dockerfile과 nginx.conf의 원리

```
# 기본
이미지: Ubuntu 22.04
FROM ubuntu:22.04

# Docker 이미지의 메인테이너를 설정 (build 시 주석으로 사용)
MAINTAINER your-name <your-email@example.com>

# 목적 라벨 (웹 서버용)
# 이 이미지는 "웹 서버" 용도로 사용됨을 라벨로 표시
LABEL purpose=Web Server

# nginx 설치: 패키지 목록 업데이트 후 nginx 설치
# apt-get을 통해 Nginx 패키지를 설치
RUN apt-get update && apt-get install -y nginx

# nginx 설정 파일을 컨테이너에 복사
# 로컬의 nginx.conf를 컨테이너의 설정 디렉토리로 복사
COPY nginx.conf /etc/nginx/nginx.conf

# nginx 실행 (백그라운드 실행 방지)
# 로컬의 nginx.conf를 컨테이너의 설정 디렉토리로 복사
CMD ["nginx", "-g", "daemon off;"]
```



- nginx.conf 설명

```
# nginx의 사용자 및 워커 프로세스 수 설정
user  nginx;
worker_processes  1;

# 에러 로그 설정(로그 파일 및 로그 레벨 정의)
error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

events {
    # 워커 프로세스 당 최대 연결 수 설정
    worker_connections  1024;
}

http {
    # 기본 MIME 타입 설정
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    # 로그의 출력 형식 정의
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    # 접근 로그 파일 위치 및 형식 설정
    access_log  /var/log/nginx/access.log  main;

    # sendfile을 통해 효율적인 파일 전송을 설정
    sendfile        on;

    # Keep-Alive 시간 설정
    # 클라이언트와 서버 간 연결 유지 시간 설정
    keepalive_timeout  65;

    # Gzip 압축 설정
    # gzip 압축을 활성화하여 데이터 전송 효율을 높임
    gzip  on;
    gzip_disable "msie6";

    # 추가 설정 파일 포함 (디폴트 설정을 추가할 때 사용)
    # 추가적인 Nginx 설정 파일들을 포함시켜서 설정을 확장 가능
    include /etc/nginx/conf.d/*.conf;
}

```