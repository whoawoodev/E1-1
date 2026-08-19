# 08. Git 설정 및 GitHub 연동

> [← 목록으로](../README.md)

---

 
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

---

> [← 목록으로](../README.md)
