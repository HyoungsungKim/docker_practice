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

