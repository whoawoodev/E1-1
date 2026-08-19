# README
<br>

## 프로젝트 개요
CLI 환경에서 터미널/Docker의 핵심 기능을 익히고, Git/GitHub과 연동하여 코드 관리부터 컨테이너 실행까지의 전 과정을 실습합니다.

##

### 리눅스/터미널 기반 개발 환경 구축

- 현재 위치 확인, 목록 확인(숨김 파일 포함), 이동, 생성, 복사, 이동/이름변경, 삭제
- 파일 내용 확인, 빈 파일 생성
- 권한 확인/변경

<br>

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
- 바인드 마운트
- 볼륨 생성, 영속성 검증

<br>

### Git 설정 및 Github 연동

- Git : 사용자 정보/기본 브랜치 설정
- Github : 로그인/저장소 연동

<br>

## 1) 실행 환경
- OS : macOS 15.7.4
- Shell : zsh
- Docker : 28.5.2
- Git : 2.53.0

<br>

## 2) 수행 체크리스트
- [x] 터미널 기본 조작 및 폴더 구성
- [x] 권한 변경 실습
- [x] Docker 설치/점검
- [x] hello-world 실행
- [x] Dockerfile 빌드/실행
- [x] 포트 매핑 접속(2회)
- [x] 바인드 마운트 반영
- [x] 볼륨 영속성
- [x] Git 설정 + VSCode GitHub 연동

<br>

 ## 3) 수행 로그

<details>
<summary>리눅스/터미널 기반 개발 환경 구축</summary>
<br>
 
- 현재 위치 확인
```bash
(사용자) ~ % pwd
```

```
/Users/(사용자)
```
<br>
<br>

- 목록확인 (숨김파일 포함)
```bash
(사용자) ~ % ls -a
```

```
.			.orbstack		.zsh_history		Downloads		OrbStack
..			.ssh			.zsh_sessions		Library			Pictures
.CFUserTextEncoding	.Trash			Desktop			Movies			Public
.docker			.vscode			Documents		Music
```
<br>
<br>
 
- 이동
```bash
(사용자) ~ % cd Documents
```

```
(사용자) Documents %
```
<br>
<br>

- 생성
```bash
(사용자) Documents % mkdir codyssey
(사용자) Documents % ls
```

```
codyssey
```
<br>
<br>

- 복사
```bash
(사용자) Documents % cp -r codyssey codyssey-test
(사용자) Documents % ls
```

```
codyssey	codyssey-test
```
<br>
<br>

- 이동/이름변경
<br>

이름변경
```bash
(사용자) Documents % mv codyssey-test codyssey-mv-test
(사용자) Documents % ls
```

```
codyssey		codyssey-mv-test
```
<br>

이동
```bash
(사용자) Documents % mv codyssey-mv-test codyssey
(사용자) Documents % ls
```

```
codyssey
```

```bash
(사용자) Documents % cd codyssey
(사용자) codyssey % ls
```

```
codyssey-mv-test
```
<br>
<br>

- 삭제
```bash
(사용자) codyssey % rm -r codyssey-mv-test
(사용자) codyssey % ls
```

```
(출력 없음 - 삭제 완료)
```
<br>
<br>

- 파일 내용 확인
```bash
(사용자) codyssey % echo "hello codyssey" > test.txt
(사용자) codyssey % cat test.txt
```

```
hello codyssey
```

```bash
(사용자) codyssey % grep "codyssey" test.txt
```

```
hello codyssey
```
<br>
<br>

- 빈 파일 생성
```bash
(사용자) codyssey % touch empty.txt
(사용자) codyssey % ls
```

```
empty.txt	test.txt
```
<br>
<br>

</details>

<details>
<summary>권한 (파일 + 디렉토리)</summary>

- 파일 생성 및 권한 확인
```bash
(사용자) codyssey % echo 'echo "hello perm"' > perm.sh && ls -l perm.sh
```
<br>
<br>

- 실행 시도 (Permission denied)
```bash
(사용자) codyssey % ./perm.sh
```
<br>
<br>

- 실행 권한 부여 후 재실행
```bash
(사용자) codyssey % chmod 755 perm.sh && ls -l perm.sh && ./perm.sh
```
<br>
<br>

- 디렉토리 권한 확인
```bash
(사용자) codyssey % mkdir permdir && ls -ld permdir
```
<br>
<br>

- 진입 권한 제거 후 cd 시도
```bash
(사용자) codyssey % chmod 600 permdir && ls -ld permdir && cd permdir
```
<br>
<br>

- 권한 복구 후 진입
```bash
(사용자) codyssey % chmod 755 permdir && ls -ld permdir && cd permdir && pwd
```
<br>
<br>

- 원래 위치로 복귀
```bash
(사용자) permdir % cd ..
```

</details>

<details>
<summary>Docker 컨테이너 워크플로우 실습</summary>

- Docker 버전 확인
```bash
(사용자) codyssey % docker --version
```
<br>
<br>

- Docker 데몬 동작 여부 확인
```bash
(사용자) codyssey % docker info
```
<br>
<br>

- 이미지 다운로드
```bash
(사용자) codyssey % docker pull nginx
```
<br>
<br>

- 이미지 목록 확인
```bash
(사용자) codyssey % docker images
```
<br>
<br>

- 컨테이너 실행
```bash
(사용자) codyssey % docker run -d -p 8090:80 --name nginx-test nginx
```
<br>
<br>

- 리소스 확인
```bash
(사용자) codyssey % docker stats --no-stream
```
<br>
<br>

- 로그 확인
```bash
(사용자) codyssey % docker logs nginx-test
```
<br>
<br>

- 컨테이너 중지
```bash
(사용자) codyssey % docker stop nginx-test
```
<br>
<br>

- 컨테이너 목록 확인
```bash
(사용자) codyssey % docker ps -a
```
<br>
<br>

- hello-world 실행 실습
```bash
(사용자) codyssey % docker run hello-world
```
<br>
<br>

- ubuntu 컨테이너 내부 명령
```bash
(사용자) codyssey % docker run -it ubuntu bash
root@[컨테이너ID]:/#
```
<br>
<br>

- OS 정보 확인
```bash
root@[컨테이너ID]:/# cat /etc/os-release
```
<br>
<br>

- 컨테이너 종료/나가기
```bash
root@[컨테이너ID]:/# exit
(사용자) codyssey %
```
<br>
<br>

- 컨테이너 유지 상태로 빠져나오기 (Detach)

```bash
(사용자) codyssey % docker run -it --name detach-test ubuntu bash
root@[컨테이너ID]:/#
```

<br>

Ctrl + P -> Ctrl + Q

<br>
<br>
<br>

- 컨테이너 종료/유지 차이
  

동작 | 명령 | 컨테이너 상태 | `docker ps`
종료 | `exit` | Exited | 안 보임 (`docker ps -a`에만 보임)
유지 | `Ctrl+P` → `Ctrl+Q` | Up | 보임

`docker run -it ubuntu bash`로 실행하면 bash가 컨테이너의 메인 프로세스가 된다.
`exit`은 그 bash를 끝내는 것이라 메인 프로세스가 사라지고 컨테이너까지 종료된다.
`Ctrl+P → Ctrl+Q`는 터미널 연결만 끊는(detach) 것이라 bash는 계속 살아 있고 컨테이너도 Up 상태로 남는다.
다시 들어갈 때는 `docker attach [이름]` 또는 `docker exec -it [이름] bash`를 사용한다.

```bash
(사용자) codyssey % docker ps -a
```
<br>
<br>

```bash
(사용자) codyssey % docker exec -it detach-test bash
```
<br>
<br>

- 실습 컨테이너 정리
```bash
root@[컨테이너ID]:/# exit
(사용자) codyssey % docker rm -f nginx-test detach-test
```
<br>
<br>

</details>

<details>
<summary>Dockerfile 커스텀 이미지 제작</summary>
 
- 선택한 방식 : (A) 웹 서버 베이스 이미지 + 정적 콘텐츠 교체
- 베이스 이미지 : `nginx:latest`
- 커스텀 포인트
  - `COPY index.html /usr/share/nginx/html/`
    - 목적 : nginx 기본 안내 페이지를 직접 작성한 페이지로 교체하여, 베이스 이미지를 그대로 쓰지 않고 내 콘텐츠가 반영된 별도 이미지를 만든다.
    - 이미지에 콘텐츠가 포함되므로 컨테이너를 몇 번 다시 실행해도 같은 결과가 재현된다.

- 작업 폴더 및 index.html 생성
```bash
(사용자) codyssey % mkdir docker-practice && cd docker-practice
```

```bash
(사용자) docker-practice % cat > Dockerfile << 'EOF'
# Nginx 기본 이미지 사용
FROM nginx:latest
# 작성한 index.html을 컨테이너 내부의 웹 서버 폴더로 복사
COPY index.html /usr/share/nginx/html/
EOF
```

```bash
(사용자) docker-practice % echo "<h1>Hello Docker! My Custom Image</h1>" > index.html
```
<br>
<br>

- Dockerfile 작성 (dockerfile 내용)
```
# Nginx 기본 이미지 사용
FROM nginx:latest
# 작성한 index.html을 컨테이너 내부의 웹 서버 폴더로 복사
COPY index.html /usr/share/nginx/html/
```
<br>
<br>

- 커스텀 이미지 빌드
```bash
(사용자) docker-practice % docker build -t my-nginx:1.0 .
```
<br>
<br>

- 빌드한 이미지로 컨테이너 실행
```bash
(사용자) docker-practice % docker run -d -p 8080:80 --name custom-web my-nginx:1.0
```
<br>
<br>

</details>

<details>
<summary>포트 매핑</summary>

- 앞 실습 컨테이너 정리
```bash
(사용자) docker-practice % docker rm -f custom-web
```
<br>
<br>

- 8080 포트로 실행
```bash
(사용자) docker-practice % docker run -d --name web-8080 -p 8080:80 my-nginx:1.0
```
<br>
<br>

- 8081 포트로 실행 (같은 이미지, 다른 포트)
```bash
(사용자) docker-practice % docker run -d --name web-8081 -p 8081:80 my-nginx:1.0
```
<br>
<br>

- 실행 중인 컨테이너 확인
```bash
(사용자) docker-practice % docker ps
```
<br>
<br>

- 접속 확인
```bash
(사용자) docker-practice % curl http://localhost:8080
```

```bash
(사용자) docker-practice % curl http://localhost:8081
```
<br>
<br>

- 실습 컨테이너 정리
```bash
(사용자) docker-practice % docker rm -f web-8080 web-8081
```
<br>
<br>

</details>

<details>
<summary>바인드 마운트</summary>

- 호스트 디렉토리 및 파일 생성
```bash
(사용자) docker-practice % mkdir bind-html && echo "<h1>Bind Mount Test - Before</h1>" > bind-html/index.html
```
<br>
<br>

- 바인드 마운트로 컨테이너 실행
```bash
(사용자) docker-practice % docker run -d --name bind-test -p 8082:80 -v $(pwd)/bind-html:/usr/share/nginx/html nginx
```
<br>
<br>

- 변경 전 응답 확인
```bash
(사용자) docker-practice % curl http://localhost:8082
```
<br>
<br>

- 호스트 파일 수정 후 응답 확인 (컨테이너 재시작 없음)
```bash
(사용자) docker-practice % echo "<h1>Bind Mount Test - After</h1>" > bind-html/index.html && curl http://localhost:8082
```
<br>
<br>

- 컨테이너 정리
```bash
(사용자) docker-practice % docker rm -f bind-test
```
<br>
<br>

</details>

<details>
<summary>볼륨 영속성</summary>
 
- Docker 볼륨 생성
```bash
(사용자) docker-practice % docker volume create my-vol
```
<br>
<br>

- 볼륨 연결하여 컨테이너 실행
```bash
(사용자) docker-practice % docker run -it --name con1 -v my-vol:/data ubuntu
root@[컨테이너ID]:/#
```
<br>
<br>

- 컨테이너 내부에서 파일 생성
```bash
root@[컨테이너ID]:/# echo "this data survives" > /data/test.txt
root@[컨테이너ID]:/# exit
```
<br>
<br>

- 컨테이너 삭제
```bash
(사용자) docker-practice % docker rm con1
```
<br>
<br>

- 같은 볼륨으로 새 컨테이너 실행
```bash
(사용자) docker-practice % docker run -it --name con2 -v my-vol:/data ubuntu
root@[컨테이너ID]:/#
```
<br>
<br>

- 데이터 유지 확인
```bash
root@[컨테이너ID]:/# cat /data/test.txt
```
<br>
<br>

- 실습 컨테이너 및 볼륨 정리
```bash
root@[컨테이너ID]:/# exit
(사용자) docker-practice % docker rm con2 && docker volume rm my-vol
```
<br>
<br>

</details>

<details>
<summary>실습 정리 (컨테이너 / 이미지)</summary>

- 남은 컨테이너 확인
```bash
(사용자) docker-practice % docker ps -a
```

```
```
<br>
<br>

- 정지된 컨테이너 일괄 삭제
```bash
(사용자) docker-practice % docker container prune -f
```

```
```
<br>
<br>

- 삭제 후 컨테이너 목록 확인
```bash
(사용자) docker-practice % docker ps -a
```

```
```
<br>
<br>

- 이미지 목록 확인
```bash
(사용자) docker-practice % docker images
```

```
```
<br>
<br>

- 실습에 사용한 이미지 삭제
```bash
(사용자) docker-practice % docker rmi my-nginx:1.0 nginx ubuntu hello-world
```

```
```
<br>
<br>

- 삭제 후 이미지 목록 확인
```bash
(사용자) docker-practice % docker images
```

```
```
<br>
<br>

</details>


<details>
<summary>Git 설정 및 GitHub</summary>
 
- 상위 폴더로 이동
```bash
(사용자) docker-practice % cd ..
```
<br>
<br>

- Git 사용자 정보 설정
```bash
(사용자) codyssey % git config --global user.name [깃허브 이름]
```

```bash
(사용자) codyssey % git config --global user.email [깃허브 이메일]
```
<br>
<br>

- 기본 브랜치 설정
```bash
(사용자) codyssey % git config --global init.defaultBranch main
```
<br>
<br>

- 설정 확인
```bash
(사용자) codyssey % git config --list
```
<br>
<br>

- 저장소 초기화
```bash
(사용자) codyssey % git init
```
<br>
<br>

- GitHub 원격 저장소 연결
```bash
(사용자) codyssey % git remote add origin [저장소 주소]
```
<br>
<br>

- 커밋 및 푸시
```bash
(사용자) codyssey % git add .
```

```bash
(사용자) codyssey % git commit -m "docs: 과제1 수행 로그"
```

```bash
(사용자) codyssey % git push -u origin main
```
<br>
<br>

</details>

<br>

 ## 4) 트러블슈팅
