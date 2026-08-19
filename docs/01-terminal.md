# 01. 터미널 기본 조작

> [← 목록으로](../README.md)

---

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

---

> [← 목록으로](../README.md)
