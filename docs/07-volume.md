# 07. 볼륨 영속성

> [← 목록으로](../README.md)

---

 
- Docker 볼륨 생성
```bash
(사용자) docker-practice % docker volume create my-vol
```

```
my-vol
```
<br>
<br>

- 볼륨 연결하여 컨테이너 실행
```bash
(사용자) docker-practice % docker run -it --name con1 -v my-vol:/data ubuntu
root@[컨테이너ID]:/#
```

```
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
06e9d71331fb: Pull complete 
f3db1cd94078: Pull complete 
Digest: sha256:6df9e8dd1eac389ebfef692c9648449adeb815d01e16e29cd6f3e50fe64ba9a6
Status: Downloaded newer image for ubuntu:latest
```
<br>
<br>

- 컨테이너 내부에서 파일 생성
```bash
root@[컨테이너ID]:/# echo "this data survives" > /data/test.txt
root@[컨테이너ID]:/# exit
```

```
exit
```
<br>
<br>

- 컨테이너 삭제
```bash
(사용자) docker-practice % docker rm con1
```

```
con1
```
<br>
<br>

- 같은 볼륨으로 새 컨테이너 실행
```bash
(사용자) docker-practice % docker run -it --name con2 -v my-vol:/data ubuntu
root@[컨테이너ID]:/#
```

이미지를 이미 받아둔 상태라 이번에는 pull 없이 바로 컨테이너에 진입했다.
<br>
<br>

- 데이터 유지 확인
```bash
root@[컨테이너ID]:/# cat /data/test.txt
```

```
this data survives
```

파일을 만든 `con1`은 이미 삭제한 상태인데도 내용이 그대로 남아 있다.
데이터가 컨테이너가 아니라 볼륨(`my-vol`)에 저장되기 때문이며, 이것이 컨테이너를 지워도 데이터가 유지되는 영속성이다.
<br>
<br>

- 실습 컨테이너 및 볼륨 정리
```bash
root@[컨테이너ID]:/# exit
(사용자) docker-practice % docker rm con2 && docker volume rm my-vol
```

```
exit
con2
my-vol
```
<br>
<br>

---

> [← 목록으로](../README.md)
