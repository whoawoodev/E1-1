# 02. 파일·디렉토리 권한

> [← 목록으로](../README.md)

---

- 파일 생성 및 권한 확인
```bash
(사용자) codyssey % echo 'echo "hello perm"' > perm.sh && ls -l perm.sh
```

```
-rw-r--r--  1 (사용자)  (사용자)  18  8 19 13:13 perm.sh
```
<br>
<br>

- 실행 시도 (Permission denied)
```bash
(사용자) codyssey % ./perm.sh
```

```
zsh: permission denied: ./perm.sh
```
<br>
<br>

- 실행 권한 부여 후 재실행
```bash
(사용자) codyssey % chmod 755 perm.sh && ls -l perm.sh && ./perm.sh
```

```
-rwxr-xr-x  1 (사용자)  (사용자)  18  8 19 13:13 perm.sh
hello perm
```
<br>
<br>

- 디렉토리 권한 확인
```bash
(사용자) codyssey % mkdir permdir && ls -ld permdir
```

```
drwxr-xr-x  2 (사용자)  (사용자)  64  8 19 13:22 permdir
```
<br>
<br>

- 진입 권한 제거 후 cd 시도
```bash
(사용자) codyssey % chmod 600 permdir && ls -ld permdir && cd permdir
```

```
drw-------  2 (사용자)  (사용자)  64  8 19 13:22 permdir
cd: permission denied: permdir
```
<br>
<br>

- 권한 복구 후 진입
```bash
(사용자) codyssey % chmod 755 permdir && ls -ld permdir && cd permdir && pwd
```

```
drwxr-xr-x  2 (사용자)  (사용자)  64  8 19 13:22 permdir
/Users/(사용자)/Documents/codyssey/permdir
```
<br>
<br>

- 원래 위치로 복귀
```bash
(사용자) permdir % cd ..
```

```
(사용자) codyssey %
```

---

> [← 목록으로](../README.md)
