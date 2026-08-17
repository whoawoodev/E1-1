# README

## 프로젝트 개요

CLI 환경에서 터미널/Docker의 핵심 기능을 익히고, Git/GitHub과 연동하여 코드 관리부터 컨테이너 실행까지의 전 과정을 실습합니다.

### 리눅스/터미널 기반 개발 환경 구축

- 현재 위치 확인, 목록 확인(숨김 파일 포함), 이동, 생성, 복사, 이동/이름변경, 삭제
- 파일 내용 확인, 빈 파일 생성
- 권한 확인/변경

### Docker 컨테이너 워크플로우 실습

- 버전 확인, 데몬 동작 확인
- 이미지 : 다운로드/목록확인
- 컨테이너 : 실행/중지/목록 확인
- 운영 : 로그 확인, 리소스 확인

- hello-world 실행
- ubuntu 컨테이너 실행, 내부 진입 후 명령 수행
- 컨테이너 종료/유지 차이 관찰

- 커스텀 이미지 제작
- 포트 매핑
- 볼륨 생성, 영속성 검증

### Git 설정 및 Github 연동

- Git : 사용자 정보/기본 브랜치 설정
- Github : 로그인/저장소 연동

## 1) 실행 환경
- OS : macOS 15.7.4
- Shell : zsh
- Docker : 28.5.2
- Git : 2.53.0

## 2) 수행 체크리스트
- [x] 터미널 기본 조작 및 폴더 구성
- [x] 권한 변경 실습
- [x] Docker 설치/점검
- [x] hello-world 실행
- [x] Dockerfile 빌드/실행
- [x] 포트 매핑 접속(2회)
- [ ] 바인드 마운트 반영
- [ ] 볼륨 영속성
- [ ] Git 설정 + VSCode GitHub 연동

 # 3) 수행 로그

<details>
<summary>리눅스/터미널 기반 개발 환경 구축</summary>
 
```
pwd
```
```
ls -a
```
```
cd
```
```
mkdir
```
```
cp -r
```
```
mv / mv -i / mv -f
```
```
rm
```
```
cat / grep "filename" file
```
```
touch
```
</details>


<details>
<summary>Docker 컨테이너 워크플로우 실습</summary>

```
docker --version, docker info
```
```
Docker version 28.5.2, build ecc6942
```
```
docker images, docker ps -a, docker logs, docker stats
```
</details>

### Git 설정 및 Github 연동
