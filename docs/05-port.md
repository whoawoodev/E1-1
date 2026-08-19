# 05. 포트 매핑

> [← 목록으로](../README.md)

---

- 앞 실습 컨테이너 정리
```bash
(사용자) docker-practice % docker rm -f custom-web
```

```
custom-web
```
<br>
<br>

- 8080 포트로 실행
```bash
(사용자) docker-practice % docker run -d --name web-8080 -p 8080:80 my-nginx:1.0
```

```
f6c82f1adb9c41fbf3a4aa66e80d1b7887f47eb84a1f4b36ab519e3cba83aaca
```
<br>
<br>

- 8081 포트로 실행 (같은 이미지, 다른 포트)
```bash
(사용자) docker-practice % docker run -d --name web-8081 -p 8081:80 my-nginx:1.0
```

```
2ca0bceb1a0983bd67b43504c669d67ab1828746ca84c597a4949ab096684dfc
```
<br>
<br>

- 실행 중인 컨테이너 확인
```bash
(사용자) docker-practice % docker ps
```

```
CONTAINER ID   IMAGE          COMMAND                   CREATED          STATUS          PORTS                                     NAMES
2ca0bceb1a09   my-nginx:1.0   "/docker-entrypoint.…"   59 seconds ago   Up 58 seconds   0.0.0.0:8081->80/tcp, [::]:8081->80/tcp   web-8081
f6c82f1adb9c   my-nginx:1.0   "/docker-entrypoint.…"   8 minutes ago    Up 7 minutes    0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   web-8080
```

같은 이미지로 만든 컨테이너 2개가 각각 호스트의 8080, 8081 포트에 연결되어 있다.
컨테이너 내부 포트는 둘 다 80으로 같지만, 컨테이너마다 네트워크가 격리되어 있어 충돌하지 않는다.
<br>
<br>

- 접속 확인
```bash
(사용자) docker-practice % curl http://localhost:8080
```

```
<h1>Hello Docker! My Custom Image</h1>
```

```bash
(사용자) docker-practice % curl http://localhost:8081
```

```
<h1>Hello Docker! My Custom Image</h1>
```
<br>
<br>

- 브라우저 접속 확인

`localhost:8080`

![8080 포트 접속](../images/포트매핑접속_8080.png)

`localhost:8081`

![8081 포트 접속](../images/포트매핑접속_8081.png)
<br>
<br>

- 실습 컨테이너 정리
```bash
(사용자) docker-practice % docker rm -f web-8080 web-8081
```

```
web-8080
web-8081
```
<br>
<br>

---

> [← 목록으로](../README.md)
