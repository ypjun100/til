# MacVLAN 네트워크

![](https://user-images.githubusercontent.com/47745785/118844433-eac9c980-b905-11eb-9366-2518a5f09d20.png)

- MacVLAN은 호스트의 네트워크 인터페이스 카드를 가상화해 물리 네트워크 환경을 컨테이너에게 제공함
- 도커 컨테이너는 물리 네트워크 상의 가상 MAC 주소와 네트워크 장비로부터 할당받은 IP를 부여함
	- 따라서, 동일 네트워크에 연결된 다른 장치와 별도 설정 없이 IP를 통해 통신이 가능해짐

> [!IMPORTANT]
> MacVLAN 네트워크를 사용하는 컨테이너는 기본적으로 호스트와 통신이 불가능하다. 위 그림에서 컨테이너 A는 서버2, 컨테이너 B와 통신할 수 있지만 서버 1과는 통신할 수 없다. 이는 기본적으로 MacVLAN 자식 인터페이스는 부모 인터페이스와 직접 통신할 수 없는 커널 제약이 존재하기 때문이다.

```shell
docker network create -d macvlan --subnet=192.168.0.0/24 \
--ip-range=192.168.0.64/28 --gateway=192.168.0.1 \
-o maclvan_mode=bridge -o parent=eth0 my_macvlan
```

- `docker network create` 명령어를 사용해 MacVLAN 네트워크를 생성할 수 있음
- 위 명령에서 사용된 옵션들은 아래와 같은 목적으로 사용됨
	- `-d`:  네트워크 드라이버로 macvlan을 사용한다는 것을 명시함
	- `--subnet`: 컨테이너가 사용할 네트워크 정보로, 네트워크 장비의 IP 대역 기본 설정을 입력함
	- `--ip-range`: MacVLAN을 생성하는 호스트에서 사용할 컨테이너의 IP 범위
	- `--gateway`: 네트워크에 설정된 게이트웨이로, 네트워크 장비의 기본 설정을 입력함
	- `-o`: 네트워크의 추가적인 옵션을 설정함