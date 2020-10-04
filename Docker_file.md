# Dockerfile

## Dockerfile을 사용한 구성 관리

### Dockerfile이란?

Docker에서는 인프라의 구성 관리를 'Dockerfile'로 기술함

- Dockerfile에는 베이스가 되는 이미지에 각종 미들웨어를 설치 및 설정하고, 개발한 애플리케이션의 실행 모듈을 전개 하기 위한 애플리케이션의 실행 기반의 모든 구성 정보를 기술함.

즉, docker에서 인프라 구성을 기술한 파일을 `Dockerfile`이라고 함.

Dockerfile은 docker 상에서 작동시킬 컨테이너의 구성 정보를 기술하기 위한 파일

- 구성 정보
  - 베이스가 될 docker 이미지
  - Docker 컨테이너 안에서 수행한 조작 (명령)
  - 환경변수 등의 설정
  - Docker 컨테이너 안에서 작동시켜둘 데몬 실행
- Docker build 명령은 Dockerfile에 기술된 구성 정보를 바탕으로 Docker 이미지를 작성 함

### Dockerfile의 기본 구문

Dockerfil은 텍스트 형식의 파일로, 확장자는 필요 없으며 `Dockerfile`이라는 이름으로 파일에 인프라의 구성 정보를 기술함.

- `Dockerfile`이외의 이름으로도 동작하지만, 이미지 빌드 할 때 명시적으로 지정해야 함.
- 명령어는 대문자든 소문자든 상관 없지만 관례적으로 대문자로 통일해서 씀.

#### 주석

```dockerfile
# 이것은 주석입니다.
```

### Dockerfile 작성

Dockfile에는 docker 컨테이너를 어떤 docker 이미지로부터 생성할지 라는 정보를 반드시 기술해야 함.

- 이 이미지를 베이스 이미지라고 함.
- 베이스 이미지는 반드시 다음 서식을 따라야 함

From 명령

```dockerfile
FROM [name of image]
FROM [name of image]:[name of tag]
FROM [name of image]:[digests]
```

- 이 `From`  명령은 필수 항목임.
- 이미지를 고유하게 특정 할 경우 digest 사용 함.
- Digest는 도커 허브에 업로드 하면 자동으로 부여되는 식별자.

Example) CentosOS의 버전 7을 베이스 이미지로 지정하여 Dockerfile 작성

```dockerfile
# 베이스 이미지 설정
FROM centos:centos7
```

## Dokerfile의 빌드와 이미지 레이어

Dockerfile을 빌드하면 dockerfile에 정의된 구성을 바탕으로 한 docker이미지를 작성 할 수 있음.

### Dockerfile로부터 Docker  이미지 만들기

```
docker build -t [name_of_image]:[name_of_tag] [Dockerfile_directory]
```

Example)

```
docker build -t sample:1.0 /home/docker/sample
```

- /home/docker/sample 경로에 있는 dockerfile을 이용하여 sample 이라는 docker 이미지 생성

### Docker  이미지의 레이어 구조

Dockerfile을 빌드하여 Docker 이미지를 작성하면 dockerfile의 명령별로 이미지를 작성함

## 명령 및 데몬 실행

### 명령 실행

```
RUN [command]
```

- Shell 형식으로 기술

Example)

```
RUN apt-get install -y nginx
```

- Exec 형식으로 기술
  - 실행하고 싶은 명령을 JSON 배열로 지정 함

 Example)

```
RUN ["/bin/bash", "-c", "apt-get install -y nginx"]
```

### 데몬 실행

- `RUN` : 이미지 작성하기 위해 실행하는 명령어 기술
- `CMD` : 이미지를 바탕으로 생성된 컨테이너 안에서 명령을 실행하려면 CMD 명령을 사용 함
  - Dockerfile에서는 하나의 명령을 기술 할 수 있음
  - 여러개를 지정하면 마지막것만 유요함

```dockerfile
CMD [command]
```

#### Exec 형식으로 기술

``` dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

#### Shell 형식으로 기술

```dockerfile
CMD nginx -g 'deamon off;'
```

### ENTRYPOINT 명령의 파라미터로 기술

`ENTRYPOINT` 명령에서 지정한 명령은 Dockerfile에서 빌드한 이미지로부터  Docker 컨테이너를 시작하기 때문에 docker container run 명령을 실행했을 때 실행 됨

```
ENTRYPOINT [command]
```

#### Exec 형식

```
ENTRYPOINT ["nginx", "-g", "daemon off;"]
```

#### Shell 형식

```
ENTRYPOINT nginx -g 'daemon off;'
```

##### ENTRYPOINT와 CMD 명령의 차이

docker container run  명령 실행 시

- ` CMD` : 컨테이너 시작 시에 실행하고 싶은 명령을 정의해도 docker container run 명령 실행 시 인수로 새로운 명령을 지정한 경우 이것을 우선 실행 함
- `ENTRYPOINT` : 명령에서 지정한 명령은 반드시 컨테이너에서 실행 됨. default 값으로 실행 하고 싶을 때 사용

Example

```dockerfile
FROM ubuntu:16.04

ENTRYPOINT ["top"]
CMD ["-d", "10"]
```

### 빌드 완료 후에 실행되는 명령(ONBUILD)

- `ONBUILD` 명령은 그 다음 빌드에서 실행할 명령을 이미지 안에 설정하기 위한 명령.
- `ONBUILD`  명령은 명령의 실행 타이밍을 늦출 수 있음.

```
ONBUILD [command]
```

- Dockerfile로부터 생성한 이미지를 베이스 이미지로 한 다른 Dockerfile을 빌드할 때 실행하고 싶은 명령을 기술 함

### 시스템 콜 시그널의 설정(STOPSIGNAL)

- 컨테이너를 종료 할 때 송신하는 시그널

```
STOPSIGNAL [signal]
```

### 컨테이너의 헬스 체크 명령(HEALTHCHECK)

```
HEALTHCHECK [option] CMD [command]
```

- 컨테이너 안의 프로세스가 정상적으로 작동하고 있는지를 체크하고 싶을 때

## 파일 설정

### 파일 및 디렉토리 추가 (ADD 명령)

```
ADD <호스트의 파일 경로> <도커 이미지의 파일 경로>
```

- `ADD ` 명령은 호스트상의 파일이나 디렉토리, 원격 파일을 Docker 이미지 안으로 복사 함.

### 파일 복사( COPY 명령)

```
COPY <호스트 파일 경로> <Docker 이미지의 파일 경로>
```

- `ADD` : 원격 파일의 다운로드나 아카이브의 압축 해제 등과 같은 기능을 가지고 있음
- `COPY` : 호스트상의 파일을 이미지 안으로 복사하는 처리만 함