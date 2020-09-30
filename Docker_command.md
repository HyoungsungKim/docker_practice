# Docker command

## Container

### Monitor process status

```
docker container ps
```

### 컨테이너 생성 및 시작

```
docker container run [option] [name_of_image][:tags] [parameters]
```

Example 1

```
docker container run -it --name "test1" centos /bin/cal
```

- `-it` : 콘솔에 결과 출력
- `--name "test1"` : 컨테이너 이름
- `centos` : 이미지명
- `\bin\cal` : 컨테이너에서 실행 할 명령

Example 2

```
docker container run -it --name "test" centos \bin\bash
```

- 도커에서 bash 실행

### 컨테이너 백그라운드 실행

```
docker container run [options] [name_of_images][:tags] [parameters]
```

Example

```
docker container run -d centod \bin\ping localhost
```

- `-d` : 백그라운드에서 실행하는 옵션

### 컨테이너의 네트워크 설정

```
docker container run [network option] [name_of_images][:tags] [parameters]
```

#### 포트 매핑

```
docker container run -d -p 8080:80 nginx
```

- `-p from:to` : mapping from host port to docker port
- `-P` : 호스트의 임의이 포트를 컨테이너에 할당

#### DNS 서버 지정

```
docker container run -d --dns 192.168.1.1 nginx
```

#### IP 주소 지정

```
docker container run -it --add-host test.com:192.168.1.1 centos
```

### 자원을 지정하여 컨테이너 생성 및 실행

```
docker container run [option] [name_of_images][:tags] [parameter]
```

Example

```
docker container run --cpu-shares=512 --memory=1g centos
```

- `--cpu-shares` : CPU의 사용률 배분
  - `512` : 기본값은 1024. 따라서 절반을 할당
- `--memory` : 사용할 메모리를 제한하여 실행

### 가동 컨테이너 목록 표시

```
docker container ls [option]
```

### 컨테이너 가동 확인

```
docker container stats [container id]
```

Example

```
docker container stats rktkek456/webserver
```

### 컨테이너 시작

```
docker container start [option] [container id 1] ...  [container id N]
```

Example

```
docker container start fff123...
```

### 컨테이너 정지 및 재시작

#### 정지

```
docker container stop [option] [contaioner id 1] ... [container id N]
```

Example

```
docker container stop -t 2 fff123...
```

- `--time` or `-t` : 컨테이너의 정지 시간을 지정
- Example 같은 경우 2초 후 정지 함

#### 재시작

```
docker container restart [option] [container id 1] .. [container id N]
```

Example

```
docker container restart -t 2 webserver
```

### 컨테이너 삭제

```
docker container rm [option] [container id 1] ... [container id N]
```

#### 정지 중인 모든 컨테이너 삭제

```
docker container prune
```

### 컨테이너 중단/재개

#### 중단

```
docker container pause [container id]
```

#### 재개

```
docker container unpause [container id]
```

## Docker container network

### 네트워크 목록 표시

```
docker network ls [option]
```

### 네트워크 작성

```
docker network create [options] [network]
```

Example 1. 브리지 네트워크 작성

```
docker network create --driver=bridge web-network
```

Example 2. 작성한 네트워크 확인

```
docker network ls --filter driver=bridge
```

### 네트워크 연결

```
docker network connect [option] [network container]
```

## 가동 중인 Docker 컨테이너 조작

### 가동 컨테이너 연결

```
docker container attach
```

### 가동 컨테이너 프로세스 실행

```
docker container exec [options] [commend] [parameters]
```

Example

```
docker container exec -it webserver /bin/echo "hello world"
```

### 가동 컨테이너의 프로세스 확인

```
docker container top [container id]
```

## Docker 이미지 생성

### 컨테이너로부터 이미지 작성

```
docker container commit [options] [container id] [image_name[:tage]]
```

