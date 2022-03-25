# AccongBox HAProxy 설정파일

### 1. HAProxy 설정파일 생성
haproxy 설정파일(haproxy.cfg)가 컨테이너가 실행될 때마다 외부 볼륨에서 설정파일을 가져오도록
홈 디렉토리에 haproxy폴더를 만들고 안에 haproxy.cfg 파일을 만들어 안에 설정을 해준다.

### 2. docker build
docker 설치 후 명령어를 통해 Dockerfile을 이용하여 이미지를 빌드한다.
```
sudo docker build -t accongbox-haproxy .
```

### 3. docker run
```
sudo docker run -d --name my-haproxy --net haproxy-net -p 80:80 -p 443:443 -p 9000:9000 --restart always -v {haproxy-디렉토리}:/usr/local/etc/haproxy accongbox-haproxy
```

- -v옵션으로 경로를 바인딩해주어 우리가 작성한 설정파일을 사용하도록 한다.
- global에 daemon옵션을 주어서 -d옵션을 줘서 백그라운트로 컨테이너를 실행한다.
- docker 컨테이너간 통신을 하기 위해서는 같은 다커 네트워크 상에 있어야 한다.
- --net 으로 해당 네트워크를 지정해준다.

haproxy.cfg 내용이 변경된다고 haproxy서버에 적용되지 않는다. 컨테이너를 재시작 해야한다.<br>
haproxy.cfg 변경 때마다 설정 문법 검증을 수행할 수 있다.
```
sudo docker exec -it my-haproxy haproxy -c -f /usr/local/etc/haproxy/haproxy.cfg
```

