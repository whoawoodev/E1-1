# 04. Dockerfile 커스텀 이미지 제작

> [← 목록으로](../README.md)

---

 
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

---

> [← 목록으로](../README.md)
