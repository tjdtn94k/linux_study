# Alpine Image Create

이 프로젝트는 Fedora 기반 이미지에서 Alpine Linux 루트 파일시스템을 포함하는 Docker 이미지를 생성하는 것을 목적으로 합니다.

## 프로젝트 구조
alpine_image_create/
├── Dockerfile # Docker 이미지 빌드 설정 파일
├── base_image_creation.txt # 이미지 생성 가이드 문서
└── alpine_root/ # Alpine Linux 루트 파일시스템 디렉토리


## 이미지 생성 과정

### 1. Fedora 베이스 이미지 준비

Fedora 패키지 업데이트
docker run --rm fedora yum update

Alpine Linux 루트 파일시스템 다운로드
wget https://github.com/alpinelinux/docker-alpine/raw/fc965e3222f368bea8e07c1c1da70b6928281a76/x86_64/alpine-minirootfs-3.15.4-x86_64.tar.gz

디렉토리 생성 및 압축 해제
mkdir alpine_root
tar zxf alpine-minirootfs-3.15.4-x86_64.tar.gz -C alpine_root/

## Dockerfile 설명
dockerfile
FROM fedora # Fedora를 베이스 이미지로 사용
COPY ./alpine_root /alpine_root # Alpine 루트 파일시스템을 /alpine_root에 복사
CMD ["/bin/sh"] # 기본 실행 명령어 설정

## 이미지 빌드 및 실행

### 이미지 빌드
docker build --tag alpine_fedora .

### 이미지 확인
docker images

### 컨테이너 실행
기본 실행
docker run -it alpine_fedora
패키지 업데이트 확인
docker run --rm alpine_fedora dnf update

## 참고 사항

- Alpine Linux 공식 이미지: https://hub.docker.com/_/alpine
- 이 프로젝트는 Fedora 베이스 이미지에 Alpine Linux 루트 파일시스템을 추가하는 방식으로 동작합니다.
- 루트 파일시스템은 버전 3.15.4를 기준으로 작성되었으며, 필요에 따라 다른 버전을 사용할 수 있습니다.

## 주의사항

1. 이미지 빌드 전 반드시 Fedora 베이스 이미지의 패키지를 업데이트
2. Alpine 루트 파일시스템의 무결성을 확인
3. 프로덕션 환경에서 사용하기 전에 보안 설정을 검토