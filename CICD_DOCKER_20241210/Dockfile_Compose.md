

## Dockfile Compose란?
- 여러 개의 Docker 컨테이너를 정의하고 관리할 수 있는 도구이다.

---
### Dockfile Compose을 사용하는 이유

  - **편하게 설정하기**: Docker Compose는 여러 컨테이너를 한 파일에 적어서 설정할 수 있다.
  - **자동으로 배포하기**: 설정 파일이 있으면, Docker Compose가 알아서 컨테이너들을 만들어 주고 실행해준다. 즉 개발자가 일일이 명령어를 입력할 필요가 없다.
  - **의존성 관리**: 컨테이너들이 서로 의존하는 관계가 있으면, Docker Compose가 이를 관리해준다. 예시로, A 컨테이너가 B 컨테이너를 필요로 하면, A를 먼저 켜고 나서 B를 실행하는 방식으로 처리된다.
  - **모니터링과 로깅**: Docker Compose는 컨테이너들이 어떻게 돌아가는지 지켜보고, 로그도 모아주기 때문에 문제가 생겼을 때 빨리 찾아서 고칠 수 있다.
  - **확장성**: 여러 컨테이너를 하나의 그룹으로 관리할 수 있기에, 웹 앱을 만드는 여러 컨테이너를 한꺼번에 관리하고 확장하기 쉽다.
  - **유연성**: Docker Compose는 개발 환경, 테스트 환경, 실제 운영 환경에서도 같은 설정 파일을 써서 일관성을 유지할 수 있다.
  - **보안 강화**: 컨테이너들의 네트워크를 분리해서 외부로부터의 접근을 제한할 수도 있다.
  - **보다 쉬운 유지보수**: 설정 파일 하나로 컨테이너들을 관리하기 때문에, 변경사항을 Docker Compose가 자동으로 적용해준다.

---
## Dockfile Compose 장점


- **한 번에 여러 컨테이너 설정하기**:
    - Docker Compose는 여러 컨테이너의 설정을 하나의 YAML 파일에 넣어서 관리한다. 이 파일 하나로 여러 컨테이너의 모든 환경을 설정하고, 그걸로 여러 컨테이너를 한 번에 실행할 수 있다.
  - **빠른 서비스 실행**:
    - 설정 값들을 저장해 두고 다시 쓸 수 있다. 만약 설정이 바뀌지 않았다면, Docker Compose는 이전에 저장해둔 정보를 다시 사용해서 서비스를 더 빨리 시작할 수 있다.
- **같은 네트워크에서 쉽게 연결**:
    - **`docker-compose.yaml`** 파일에 있는 애플리케이션들은 모두 같은 네트워크에 자동으로 연결된다. 이렇게 하면 복잡한 네트워크 설정 없이도 여러 컨테이너가 서로 쉽게 통신할 수 있다.
    
설정도 간편하고, 빠르게 실행할 수 있고, 컨테이너들끼리의 연결도 쉽게 할 수 있기에 **Dockfile Compose** 는 개별장 여러 컨테이너를 관리하고 실행하는 데 정말 편리한 도구이다.

---
## Dockfile Compose 사용하는 위치
- **개발 환경에서**
    - 앱을 개발할 때, 앱을 따로 떼어 놓고 실행하고 테스트할 수 있는 환경이 필요하다. Docker Compose를 쓰면 이런 환경을 쉽게 만들고 관리할 수 있다.
    - Compose 파일은 앱이 필요로 하는 모든 서비스들(데이터베이스, 큐, 캐시, 웹 API 등)을 정리해주고, **`docker compose up`** 명령어로 이 모든 것을 한 번에 시작할 수 있다.
    - 이런 기능들 덕분에 개발자가 새 프로젝트를 시작할 때 시간을 많이 절약할 수 있다. 여러 페이지에 걸친 설명서 대신에 Compose 파일 하나로 모든 설정을 할 수 있기 때문이다.
- **자동화된 테스트 환경에서**
    - 자동화된 테스트는 앱이 잘 돌아가는지 확인하는 데 중요하다. Docker Compose는 이런 테스트를 위한 별도의 환경을 쉽게 만들고 없앨 수 있다.
    - Compose 파일에 테스트 환경을 정의해두고, 간단한 명령어 몇 개로 테스트 환경을 만들고 테스트를 실행한 다음, 다시 환경을 없앨 수 있다. 예를 들어 다음과 같은 명령어를 사용하면 된다.
    ```
    docker compose up -d
    ./run_tests
    docker compose down
    ```
- **단일 호스트 배포에서**
    - Docker Compose는 주로 개발과 테스트에 많이 쓰이지만, 실제로 앱을 운영하는 환경(프로덕션)에도 쓸 수 있다. 매번 새 버전이 나올 때마다 이런 용도로도 쓰기 좋게 계속 개선되고 있다.



---
### Dockerfile Compose 실행하는 법

1. 각 애플리케이션의 Dockerfile 작성하기
  - 보통 내가 만든 애플리케이션을 실행하기 위한 Dockerfile 만 작성
2. docker-compose.yaml 파일 작성하기
   - 내가 만든 애플리케이션을 실행하기 위해 필요한 database라든지 redis라든지 다른 서비스들을 한꺼번에 정의하는 파일을 작성
3. `docker compose up` 으로 실행하기


- #### YAML 파일이란?
  - 'YAML Ain't Markup Language(마크업 언어가 아니다)'의 줄임말이며, YAML 파일은 컴퓨터가 읽을 수 있는 설정 파일이다. 사람이 읽기에도 쉬운 텍스트 형식으로 되어 있다.
  - YAML 파일은 일반 텍스트로 쓰여 있다. 목록이나 키-값 쌍 등으로, 설정이나 데이터를 쉽게 알아볼 수 있는 형식으로 나열한다. 
  - YAML 파일은 설정을 정리하고 관리하기에 유용하게 사용한다.. 예를 들어, Docker Compose에서는 YAML 파일을 사용해서 여러 컨테이너의 설정을 한 곳에 쉽게 정리할 수 있다.
  - YAML은 읽기 쉽고, 쓰기도 간단해서 많은 프로그램과 도구에서 선호하는 설정 파일 형식이다.
  - YAML 파일은 구조가 명확하고 간단해서, 사람이 보기에도 이해하기 쉬운 장점이 있다. 들여쓰기를 사용해서 각 설정의 관계를 나타낸다.

- #### YAML 문법

  - **키-값 쌍**: 키와 값으로 이루어진 쌍으로 구성된다. 키와 값은 콜론(:)으로 구분한다.
  - **리스트**: 쉼표(,)로 구분된 값들의 리스트로 구성된다.
  - **딕셔너리**: 중괄호({})로 둘러싸인 키-값 쌍의 리스트로 구성된다.
  - **불린 값**: true, false, yes, no 등의 값으로 표현된다.
  - **문자열**: 큰 따옴표("")나 작은 따옴표('')로 둘러싸인 문자열로 표현된다.

  - **주의사항**
    - 보통 들여쓰기가 잘못된 경우 yaml 파일을 의도와 다르게 해석하게 되니 들여쓰기에 주의할 것.
    - https://www.yamllint.com/ 에서 들여쓰기를 검사할 수 있다.

---
### Docker compose 파일 형태 설명

```
version: '3' # Docker Compose의 버전을 정의
services: # 4개(web,api,redis,mysql)의 서비스 정의
  web: # 이름 지정 가능, `web` 서비스는 Nginx 이미지를 사용하며, 포트 80번을 호스트 머신에 노출
    image: nginx:latest
    ports: # 앞 포트에서 뒤 포트로 연결
      - 80:80
    volumes: # `web` 서비스는 현재 디렉토리 내 `web` 디렉토리를 컨테이너에 연결
      - ./web:/usr/share/nginx/html
    depends_on: # 각 서비스 간의 의존성을 정의, `web` 서비스는 `api`서비스에 의존
      - api
    links: # 각 서비스 간의 링크를 정의, `web` 서비스는 `api`서비스에 링크됨
      - api:api
  api: # `api` 서비스는 Java 이미지를 사용하며, 포트 8080번을 호스트 머신에 노출
    image: java:latest
    volumes: # `api` 서비스는 현재 디렉토리 내 `api` 디렉토리를 컨테이너에 연결
      - ./api:/app
    ports:
      - 8080:8080
    environment:
      - REDIS_HOST=redis
      - MYSQL_HOST=mysql
      - MYSQL_USER=root
      - MYSQL_PASSWORD=password
      - MYSQL_DATABASE=test
    depends_on:  # 각 서비스 간의 의존성을 정의, #`api` 서비스는 `mysql`과 `redis` 서비스에 의존
      - mysql
      - redis
    links: # 각 서비스 간의 링크를 정의, # `api` 서비스는 `mysql`과 `redis` 서비스에 링크됨
      - mysql:mysql
      - redis:redis
  redis: # `redis` 서비스는 Redis 이미지를 사용하며, 포트 6379번을 호스트 머신에 노출 (volumes 없음)
    image: redis:latest
    ports:
      - 6379:6379
  mysql: # `mysql` 서비스는 MySQL 이미지를 사용하며 포트 3306번을 호스트 머신에 노출
    image: mysql:latest
    ports:
      - 3306:3306
    volumes: # `mysql` 서비스는 현재 디렉토리 내 `mysql` 디렉토리를 사용하여 MySQL 데이터를 영구적으로 저장
      - ./mysql:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=password
      - MYSQL_DATABASE=test
      - MYSQL_USER=root
      - MYSQL_PASSWORD=password
```
이 예제에서는 `web` 서비스와 `api` 서비스가 각각 독립적인 컨테이너로 실행된다. `web` 서비스는 `api` 서비스에 의존하며, `api` 서비스는 `mysql`과 `redis` 서비스에 의존하며, `redis`와 `mysql` 서비스는 각각 독립적인 컨테이너로 실행된다.

---
```
version: "3"
services:
	service1:
		# ...service1 설정
  service2:
    # ...service2 설정
networks:
	# 네트워크 설정, 선택적
volumes:
  # 볼륨 설정, 선택적
```

- service1, service2 각각에 서비스로 부를 컨테이너에 대한 정보를 입력한다. 컨테이너 대신 서비스 개념으로 간주한다.
- 네크워크, volume 를 별도로 지정하는데, 이는 생략해도 된다.

----
### Docker Engine 
- 참고 사이트 : https://docs.docker.com/compose/compose-file/compose-versioning/

- 보통 docker engine 3.8로 사용한다. 다음과 같은 형식으로 사용한다.

```
web (이름 지정 가능) : 

build
    context
        dockerfile
        이거 대신에 만들어진 image pull해서 사용 ok


services:
  web:
    build:
      context: .    # Dockerfile 의 위치
      dockerfile: Dockerfile    # Dockerfile 파일명
    container_name: testapp_web_1    # 생략하는 경우 부여되는 이름, 애초에 지정해서 이름변경 가능
    # 자동으로 부여 docker run 의 --name 옵션과 동일
    ports: "8080:8080"  # docker run 의 -p 옵션으로 docker run 한 것과 동일
    expose: "8080"    # 호스트머신과 연결이 아니라 
    # 링크로 연결된 서비스 간 통신이 필요할 때 사용되는 포트
    networks: testnetwork    # networks 를 최상위에 정의한다면 해당 이름을 사용
    # docker run의 --net 옵션과 동일
    volumes: .:/var/lib/nginx/html    # docker run 의 -v 옵션과 동일 # 현재 디렉토리를 html에 연결
    environment:
      - APPENV=TEST    # docker run 의 -e옵션과 동일
    command: npm start   # docker run 의 가장 마지막, 이 container를 실행 시 어떤 명령어를 실행할 지 작성
    restart: always    # docker run 의 --restart 옵션과 동일
    depends_on: db    # 이 옵션에 지정된 서비스가 시작된 이후에 `web`서비스가 실행
    links: db # Docker가 네트워크를 통해 컨테이너를 연결하도록 정의합
    # 컨테이너를 연결할 때 Docker는 환경 변수를 만들고 
    # 컨테이너를 알려진 호스트 목록에 추가하여 서로를 검색 가능
    deploy:    # 서비스의 복제본 개수 등 지정
      replicas: 3
      mode: replicated

```
- `volumes`:
    - 도커 볼륨 혹은 호스트 볼륨을 마운트하여 사용한다. 도커 볼륨의 경우 `docker-compose.yml` 파일에 선언된 볼륨만 `docker-compose.yml`에서 사용할 수 있다.
```
version: "3.9"
services:
  web:
    # ...
    volumes:
      - README.md:/docs/README.md # 호스트의 README.md 파일을 컨테이너 내부 /docs/README.md에 마운트   
      - logvolume01:/var/log # 선언된 도커 볼륨 logvolume01을 컨테이너 내부 /var/log에 마운트
# ...
volumes:
  logvolume01: {} # 도커볼륨 logvolume01 선언
```

- `networks`:
    - 서비스(컨테이너)가 소속된 네트워크이다. 따로 지정하지 않을 경우 default_${project}와 같이 지정된다. 기본적으로 컨테이너는 같은 네트워크에 있어야 서로 통신이 가능하다.
- `healthcheck`
    - 서비스 컨테이너가 “healthy”한지 계속 체크한다.
    - Dockerfile에 정의된 것을 먼저 따르지만 docker-compose 파일에서 재지정 가능하다
    - 예제
    ```
    healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost"]
  interval: 1m30s
  timeout: 10s
  retries: 3
  start_period: 40s
  start_interval: 5s
  ```

----
### Docker Compose CLI(Command Line Interface)
Docker Compose 도구와 관련된 명령어를 실행할 수 있는 명령줄 인터페이스

- `docker-compose [COMMAND] [SERVICES...]`의 형태로 지정된 서비스(컨테이너)만 제어가 가능한다. 예를 들어서 web, redis 중에 web만 기동하고 싶을 경우 `docker-compose up -d web`와 같이 실행한다.
- `docker-compose up`
    - docker-compose up 실행시 다음의 순서로 진행한다. 이미 생성된 경우 해당 단계를 건너뛴다. (멱등성)
    1. 서비스를 띄울 네트워크 생성
    2. 필요한 볼륨 생성(혹은 이미 존재하는 볼륨과 연결)
    3. 필요한 이미지 풀(pull)
    4. 필요한 이미지 빌드(build)
    5. 서비스 실행 (depends_on 옵션 사용시 서비스 의존성 순서대로 실행)
- `--build`
    - 이미 빌드가 되었더라도 강제로 빌드를 진행한다.
- -`d`
    - 백그라운드로 실행한다.
- `--force-recreate`
    - docker-compose.yml 파일의 변경점이 없더라도 강제로 컨테이너를 재생성한다. 다시 말해서 컨테이너가 종료되었다가 다시 생성된다.
- `docker-compose down`
    - 서비스를 멈추고 삭제한다. 컨테이너와 네트워크를 삭제한다.
- `--volume`
    - 선언된 도커 볼륨도 삭제한다.
- docker-compose stop, docker-compose start
    - 서비스를 멈추거나, 멈춰 있는 서비스를 시작한다.
- docker-compose ps
    - 현재 환경에서 실행 중인 각 서비스의 상태를 표시한다.
- docker-compose logs
    - 컨테이너 로그를 확인한다.
- `-f`
    - tail -f와 유사하게 컨테이너 로그를 실시간으로 확인한다. (follow)
- `docker-compose exec`
    - 실행 중인 컨테이너에 해당 명령어를 실행한다.
    ```
    docker-compose exec django ./manage.py makemigrations
    docker-compose exec db psql postgres postgres
    ```
- `docker-compose run`
  - 특정 명령어를 일회성으로 실행하지만 컨테이너를 batch성 작업으로 사용하는 경우에 해당한다. 이미 기동하고 있는 컨테이너에 명령어를 실행하고자 하면 docker-compose exec을 사용하는 반면에 docker-compose run을 사용할 경우 컨테이너를 기동시키고 특정 명령어를 실행이 완료된 후에 컨테이너를 종료한다.
  ```
  docker-compose exec web echo "hello world" # 이미 실행된 web 컨테이너에서 echo "hello world"를 실행
  docker-compose run web echo "hello world" # web 컨테이너에서 echo "hello world"를 실행하고 컨테이너 종료
  ```
