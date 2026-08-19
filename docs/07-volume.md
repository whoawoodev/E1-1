# 07. 볼륨 영속성

> [← 목록으로](../README.md)

---

 
- Docker 볼륨 생성
```bash
(사용자) docker-practice % docker volume create my-vol
```
<br>
<br>

- 볼륨 연결하여 컨테이너 실행
```bash
(사용자) docker-practice % docker run -it --name con1 -v my-vol:/data ubuntu
root@[컨테이너ID]:/#
```
<br>
<br>

- 컨테이너 내부에서 파일 생성
```bash
root@[컨테이너ID]:/# echo "this data survives" > /data/test.txt
root@[컨테이너ID]:/# exit
```
<br>
<br>

- 컨테이너 삭제
```bash
(사용자) docker-practice % docker rm con1
```
<br>
<br>

- 같은 볼륨으로 새 컨테이너 실행
```bash
(사용자) docker-practice % docker run -it --name con2 -v my-vol:/data ubuntu
root@[컨테이너ID]:/#
```
<br>
<br>

- 데이터 유지 확인
```bash
root@[컨테이너ID]:/# cat /data/test.txt
```
<br>
<br>

- 실습 컨테이너 및 볼륨 정리
```bash
root@[컨테이너ID]:/# exit
(사용자) docker-practice % docker rm con2 && docker volume rm my-vol
```
<br>
<br>

---

> [← 목록으로](../README.md)
