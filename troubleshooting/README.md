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

## 문제 2 — (제목)

**증상**

```
(에러 메시지 또는 예상과 다른 동작)
```

**가설**

**확인**

```
(확인에 사용한 명령과 출력)
```

**조치**

```
(해결한 명령과 출력)
```

**정리**

---

> [← 목록으로](../README.md)
