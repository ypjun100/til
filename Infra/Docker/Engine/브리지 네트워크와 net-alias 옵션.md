# 브리지 네트워크와 net-alias 옵션

```shell
docker run -itd --net mybridge --netalias netgroup ubuntu
docker run -itd --net mybridge --netalias netgroup ubuntu
docker run -itd --net mybridge --netalias netgroup ubuntu

docker run -it --net mybridge ubuntu
$ ping -c 1 netgroup # 172.18.0.5 컨테이너에 연결됨
$ ping -c 1 netgroup # 172.18.0.3 컨테이너에 연결됨
$ ping -c 1 netgroup # 172.18.0.4 컨테이너에 연결됨
```

- 브리지 네트워크와 `run` 명령어의 `--net-alias`를 쓰면, 특정 이름으로 여러 컨테이너에 접근 가능함
	- 위 예시에서는 `netgroup`이라는 호스트 이름으로 3개의 컨테이너에 라운드 로빈으로 접근할 수 있음
- 도커 내장 DNS가 `netgroup`이라는 호스트명에 대해 `--net-alias`를 설정한 컨테이너 주소로 변환함
	- 도커 내장 DNS는 호스트 이름으로 IP가 유동적으로 변경되는 컨테이너를 손쉽게 찾을 때 주로 사용됨

![](https://user-images.githubusercontent.com/47745785/118841704-7ee66180-b903-11eb-9b7c-1937e3edc2af.png)

> [!IMPORTANT]
> 도커 컨테이너에서 호스트명 DNS 해석은 `도커 내장 DNS`, `호스트 DNS`, `외부 DNS` 순으로 질의한다. **이때 기본 브리지 네트워크(`docker0`)은 도커 내장 DNS를 거치지 않는다.**
> 
> 컨테이너를 `docker run`으로 실행하면 기본 `docker0` 브리지 네트워크에 연결되는데, 이 네트워크에서 도커는 컨테이너의 `/etc/resolv.conf`에 호스트의 DNS 설정만 그대로 복사한다. 기본 브리지 네트워크의 경우 레거시 방식이며, 레거시 방식에는 내장 DNS가 없어서, 발생하는 이슈이다.
> - `/etc/resolv.conf`은 리눅스 시스템에서 DNS 쿼리를 어느 서버로 보낼지를 지정하는 설정 파일