# 트러블슈팅

> [← 목록으로](../README.md)

실습 중 발생한 문제와 해결 과정을 기록한다. 각 문제는 **증상 → 가설 → 확인 → 조치** 순서로 정리한다.

---

## 문제 1 — git push 인증 실패 (Password authentication is not supported)

**증상**

`git push -u origin main` 실행 시 사용자명과 비밀번호를 입력했으나 인증에 실패했다.

```
Username for 'https://github.com': [깃허브 이름]
Password for 'https://[깃허브 이름]@github.com': 

remote: Invalid username or token. Password authentication is not supported for Git operations.
fatal: Authentication failed for '[저장소 주소]'
```

**가설**

계정 비밀번호가 틀렸을 가능성과, GitHub이 HTTPS 방식의 Git 작업에서 비밀번호 인증 자체를 받지 않을 가능성 두 가지를 생각했다.

**확인**

에러 메시지에 `Password authentication is not supported for Git operations`라고 명시되어 있다.
비밀번호가 틀렸다면 `Invalid username or password`가 나올 텐데, 지원하지 않는다(`not supported`)고 답한 것이므로 비밀번호의 정오 문제가 아니라 인증 방식 자체가 거부된 것이다.
즉 HTTPS로 push하려면 비밀번호가 아니라 토큰이나 그에 준하는 자격 증명이 필요하다.

**조치**

Personal Access Token을 직접 발급해 입력하는 방법도 있으나, VSCode의 GitHub 로그인을 사용했다.
VSCode에서 로그인하면 발급된 자격 증명이 `osxkeychain`에 저장되고, `git config --list`에서 확인했듯 `credential.helper=osxkeychain`이 이미 설정되어 있으므로 터미널의 `git` 명령도 같은 자격 증명을 사용한다.

로그인 후 같은 명령을 다시 실행하니 성공했다.

```
오브젝트 나열하는 중: 4, 완료.
오브젝트 개수 세는 중: 100% (4/4), 완료.
Delta compression using up to 6 threads
오브젝트 압축하는 중: 100% (3/3), 완료.
오브젝트 쓰는 중: 100% (4/4), 475 bytes | 475.00 KiB/s, 완료.
Total 4 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To [저장소 주소]
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

**정리**

토큰을 따로 발급해 관리하지 않아도, 자격 증명 저장소(`credential.helper`)를 공유하는 도구로 한 번 로그인하면 터미널까지 함께 인증된다.
GUI에서 로그인했는데 CLI가 통과되는 이유가 여기에 있다.

관련 로그 : [08. Git 설정 및 GitHub 연동](../docs/08-git-github.md)

---

## 문제 2 — 바인드 마운트 후 컨테이너가 모듈을 찾지 못함 (Cannot find module)

**증상**

06에서 확인한 방식대로 소스 디렉토리를 바인드 마운트하고 컨테이너를 실행하니, 이미지 빌드 시 설치한 패키지를 찾지 못하고 `Error: Cannot find module 'express'`로 종료했다. 빌드 로그에는 설치가 정상적으로 표시된 상태였다.

사용한 구성은 다음과 같다.

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
CMD ["node", "index.js"]
```

```yaml
services:
  app:
    build: .
    volumes:
      - ./:/app
```

**가설**

두 가지를 생각했다.

1. `npm install`이 실제로는 실패했고 빌드 로그만 성공처럼 보였을 가능성
2. 바인드 마운트가 이미지 안 `node_modules`를 가렸을 가능성

1이라면 마운트 여부와 무관하게 컨테이너 안에 `node_modules`가 없어야 하고, 2라면 마운트를 걸지 않았을 때는 존재해야 한다. 두 경우를 구분할 수 있으므로 같은 이미지를 마운트 없이 한 번, 마운트를 걸고 한 번 실행해 비교했다.

**확인**

```bash
docker run --rm <이미지> ls /app/node_modules              # 마운트 없음
docker run --rm -v "$(pwd)":/app <이미지> ls /app/node_modules   # 마운트 있음
```

마운트를 걸지 않았을 때는 설치된 패키지가 그대로 나오고, 걸었을 때만 사라진다. 호스트 디렉토리에는 `node_modules`가 없으므로 컨테이너 입장에서도 없는 것이 된다. 설치가 실패한 것이 아니라 설치 결과가 가려진 것이므로 가설 2가 맞다.

마운트는 컨테이너의 해당 경로를 호스트 디렉토리로 대체한다. 06에서 호스트의 파일 수정이 컨테이너에 즉시 반영된 것도 같은 원리인데, `/app` 전체를 대체하므로 그 아래의 `node_modules`까지 함께 가려진 것이다.

**조치**

가려진 경로만 다시 덮어썼다. 마운트는 경로가 더 깊은 쪽이 우선하므로, `/app`에 바인드가 걸려 있어도 `/app/node_modules`에 건 마운트가 그 부분을 다시 채운다.

```yaml
services:
  app:
    build: .
    volumes:
      - ./:/app
      - node_modules:/app/node_modules

volumes:
  node_modules:
```

07에서 확인했듯 비어 있는 볼륨을 마운트하면 이미지 안 해당 경로의 내용이 볼륨으로 복사된다. 따라서 소스는 호스트의 것을, `node_modules`는 이미지의 것을 사용하게 되고 컨테이너가 정상 기동한다.

컨테이너 경로만 적는 익명 볼륨(`- /app/node_modules`)으로도 같은 결과가 나오지만, `docker volume ls`에 해시 문자열로만 표시되어 개별 관리가 어렵다.

이후 `package.json`에 패키지를 추가하고 다시 빌드했을 때 반영되지 않는 일이 있었다. 볼륨의 수명이 컨테이너와 분리되어 있어 이전 내용이 그대로 남기 때문으로, 07에서 컨테이너를 삭제한 뒤에도 데이터가 유지된 것과 같은 성질이다. 볼륨까지 함께 정리하면 다음 실행에서 다시 복사된다.

```bash
docker compose down -v
```

**정리**

바인드 마운트는 호스트의 내용을 반영하는 동시에 컨테이너 원래의 내용을 가린다. 06에서는 이 성질이 목적에 맞게 작동했지만, 가려지면 안 되는 디렉토리가 그 아래에 있으면 문제가 된다.

호스트에 `node_modules`를 만들어 두는 방법은 해결책이 되지 않는다. macOS에서 설치한 네이티브 모듈은 리눅스 컨테이너에서 동작하지 않기 때문으로, 실행 환경이 다르면 설치 결과도 달라야 한다. 볼륨과 바인드 마운트를 한 경로에 겹쳐 쓰는 방식은 소스는 공유하되 설치 결과는 컨테이너 안에 두기 위한 것이다.

파이썬의 가상환경(`.venv`), Go의 빌드 캐시, Rust의 `target/`처럼 실행 환경에 종속된 디렉토리가 마운트 경로 아래에 있을 때 같은 문제가 발생한다.

관련 로그 : [06. 바인드 마운트](../docs/06-bind-mount.md) · [07. 볼륨 영속성](../docs/07-volume.md)

---

> [← 목록으로](../README.md)
