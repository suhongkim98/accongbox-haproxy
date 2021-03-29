# accongbox-haproxy
haproxy 설정파일

```
1. 먼저 Docker 설치 후 docker pull 명령어로 haproxy를 받아온다.

2. haproxy 설정파일(haproxy.cfg)가 컨테이너가 실행될 때마다 외부 볼륨에서 설정파일을 가져오도록
홈 디렉토리에 haproxy폴더를 만들고 안에 haproxy.cfg 파일을 만들어 안에 설정을 해준다.

3. 컨테이너 실행 시 sudo docker run -d --name my-haproxy -v /root/haproxy:/usr/local/etc/haproxy haproxy:latest
와 같은 형태로 경로를 바인딩해주어 컨테이너 외부의 설정파일을 적용한다. 
global에 daemon옵션을 주어서 -d옵션을 줘서 백그라운트로 컨테이너를 실행한다.

나는 sudo docker run -d --name my-haproxy -p 80:80 -v /root/haproxy:/usr/local/etc/haproxy haproxy:latest 처럼 포트포워딩을 해주어서 성공했다.

4. haproxy.cfg 내용이 변경된다고 haproxy서버에 적용되지 않는다.
sudo docker exec -it my-haproxy haproxy -c -f /usr/local/etc/haproxy/haproxy.cfg
와 같은 꼴로 haproxy.cfg 변경 때마다 검증 수행해서 적용하자.
```
