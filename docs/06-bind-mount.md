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

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
26c307b5e35a: Already exists 
3c55dc422a81: Already exists 
d84ae7b21412: Already exists 
c0df8d325117: Already exists 
b8b80b9bc028: Already exists 
f5de6e85ac74: Already exists 
5a4222b844e8: Already exists 
Digest: sha256:8541484afbc9c8a5a8a99b379568ebbc957f658583ec9448fc43104229c03cf8
Status: Downloaded newer image for nginx:latest
27d03e8eb134a20e46c0ba34d74546eb057f1bd21605669542876eb83a7b6601
```

커스텀 이미지가 아닌 nginx 공식 이미지를 그대로 사용했다.
04에서 `my-nginx:1.0`을 만들 때 이미 같은 레이어를 받아둔 상태라 `Already exists`로 표시되고 실제 다운로드는 일어나지 않았다.
<br>
<br>

- 변경 전 응답 확인
```bash
(사용자) docker-practice % curl http://localhost:8082
```

```
<h1>Bind Mount Test - Before</h1>
```
<br>
<br>

- 호스트 파일 수정 후 응답 확인 (컨테이너 재시작 없음)
```bash
(사용자) docker-practice % echo "<h1>Bind Mount Test - After</h1>" > bind-html/index.html && curl http://localhost:8082
```

```
<h1>Bind Mount Test - After</h1>
```

컨테이너를 다시 빌드하거나 재시작하지 않았는데도 응답이 바뀌었다.
바인드 마운트는 호스트 디렉토리를 컨테이너 내부 경로에 그대로 연결하는 방식이라, 호스트에서 파일을 고치면 컨테이너가 보는 파일도 같이 바뀐다.
04의 `COPY`는 빌드 시점에 이미지 안으로 복사하는 것이라 파일을 고치면 다시 빌드해야 하는 것과 대비된다.
<br>
<br>

- 컨테이너 정리
```bash
(사용자) docker-practice % docker rm -f bind-test
```

```
bind-test
```
<br>
<br>

---

> [← 목록으로](../README.md)
