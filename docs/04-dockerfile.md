# 04. Dockerfile 커스텀 이미지 제작

> [← 목록으로](../README.md)

---

 
- 선택한 방식 : (A) 웹 서버 베이스 이미지 + 정적 콘텐츠 교체
- 베이스 이미지 : `nginx:latest`
- 커스텀 포인트
  - `COPY index.html /usr/share/nginx/html/`
    - 목적 : nginx 기본 안내 페이지를 직접 작성한 페이지로 교체하여, 베이스 이미지를 그대로 쓰지 않고 내 콘텐츠가 반영된 별도 이미지를 만든다.
    - 이미지에 콘텐츠가 포함되므로 컨테이너를 몇 번 다시 실행해도 같은 결과가 재현된다.

- 작업 폴더 · Dockerfile · index.html 생성
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

- 커스텀 이미지 빌드
```bash
(사용자) docker-practice % docker build -t my-nginx:1.0 .
```

```
[+] Building 7.6s (7/7) FINISHED                                                docker:orbstack
 => [internal] load build definition from Dockerfile                                       0.2s
 => => transferring dockerfile: 205B                                                       0.0s
 => [internal] load metadata for docker.io/library/nginx:latest                            2.3s
 => [internal] load .dockerignore                                                          0.1s
 => => transferring context: 2B                                                            0.0s
 => [internal] load build context                                                          0.2s
 => => transferring context: 76B                                                           0.0s
 => [1/2] FROM docker.io/library/nginx:latest@sha256:8541484afbc9c8a5a8a99b379568ebbc957f658583ec9448fc43104229c03cf8    4.0s
 => => resolve docker.io/library/nginx:latest@sha256:8541484afbc9c8a5a8a99b379568ebbc957f658583ec9448fc43104229c03cf8    0.2s
 => => sha256:963cfe6e75d1c292f66589d7e190b137cf89310414c0c1c5b476dfc61a4fcd0d 2.29kB / 2.29kB     0.0s
 => => sha256:5253dc86cc93ac6249902934655c6f7c959d8caa45a8c2ecc0b95953834d8ee8 9.09kB / 9.09kB     0.0s
 => => sha256:26c307b5e35a59ce911f5fde5b9458120ec8734e831ea2da5649a9ad14abfd3d 29.78MB / 29.78MB   0.8s
 => => sha256:8541484afbc9c8a5a8a99b379568ebbc957f658583ec9448fc43104229c03cf8 10.23kB / 10.23kB   0.0s
 => => sha256:3c55dc422a8172d2ad90565ea86c2211e5d9b1c854db7ecf506ece263e0d3fe6 33.33MB / 33.33MB   1.0s
 => => sha256:d84ae7b21412e1d7bd1241050a967db4a0d7c24ee83ecb1f21ee231bb2e07d92 628B / 628B         0.7s
 => => sha256:c0df8d325117373948c15350cc4887825c8708961670514c384a9f6ba86403ae 954B / 954B         1.0s
 => => sha256:b8b80b9bc028c336675d1bfb9d8fd56e379e3f1c8c6dcce69693901d9689a1b5 403B / 403B         1.1s
 => => extracting sha256:26c307b5e35a59ce911f5fde5b9458120ec8734e831ea2da5649a9ad14abfd3d          1.0s
 => => sha256:f5de6e85ac74fc63dae94ac7ac121d8e3755f1743efac65eeaef983b35767b24 1.21kB / 1.21kB     1.2s
 => => sha256:5a4222b844e843499b76e3eb9f0088b1812e432c6965a1f50d48efc4d99cd0c9 1.40kB / 1.40kB     1.3s
 => => extracting sha256:3c55dc422a8172d2ad90565ea86c2211e5d9b1c854db7ecf506ece263e0d3fe6          0.7s
 => => extracting sha256:d84ae7b21412e1d7bd1241050a967db4a0d7c24ee83ecb1f21ee231bb2e07d92          0.0s
 => => extracting sha256:c0df8d325117373948c15350cc4887825c8708961670514c384a9f6ba86403ae          0.0s
 => => extracting sha256:b8b80b9bc028c336675d1bfb9d8fd56e379e3f1c8c6dcce69693901d9689a1b5          0.0s
 => => extracting sha256:f5de6e85ac74fc63dae94ac7ac121d8e3755f1743efac65eeaef983b35767b24          0.0s
 => => extracting sha256:5a4222b844e843499b76e3eb9f0088b1812e432c6965a1f50d48efc4d99cd0c9          0.0s
 => [2/2] COPY index.html /usr/share/nginx/html/                                            0.4s
 => exporting to image                                                                     0.2s
 => => exporting layers                                                                    0.1s
 => => writing image sha256:ed0bd0ede8c7c4f801768fe43b4d52902cfbab2632d094c75fd7acb4ebaf91cf      0.0s
 => => naming to docker.io/library/my-nginx:1.0                                            0.0s
```
<br>
<br>

- 빌드한 이미지로 컨테이너 실행
```bash
(사용자) docker-practice % docker run -d -p 8080:80 --name custom-web my-nginx:1.0
```

```
fcde83c96b53f12c8c5a9910210ea35af5a984ded6b682126195774a2a054063
```
<br>
<br>

---

> [← 목록으로](../README.md)
