# 06. 바인드 마운트

> [← 목록으로](../README.md)

---

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

---

> [← 목록으로](../README.md)
