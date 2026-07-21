# YAML 파일 작성

- 도커 컴포즈의 YAML 파일은 크게 `서비스 정의`, `볼륨 정의`, `네트워크 정의`의 3가지 항목으로 구성됨
	- 여기서 `서비스 정의`는 필수 값이고, 나머지는 모두 생략 가능함
- 각 항목의 하위 항목을 정의하려면 2개의 스페이스로 들여쓰기해서 상위 항목과 구분함

> 도커 컴포즈는 기본적으로 현재 디렉토리 또는 상위 디렉토리에서 `docker-compose.yml` 파일을 찾아 컨테이너를 생성한다. 이때 `docker compose` 명령어의 `-f` 옵션을 사용하면 yml 파일의 위치와 이름을 지정할 수 있다.
> ```
> docker compose -f custom-compose.yml up -d
> ```

### 서비스 정의

```
services:
  my_container_1:
    image: ...
  my_container_2:
    image: ...
```

- 이 항목에 쓰인 각 서비스는 컨테이너로 구현되며, 하나의 프로젝트로서 도커 컴포즈에 의해 관리됨
- 서비스 이름은 services의 하위 항목으로 정의하고, 컨테이너 옵션은 서비스 이름의 하위 항목에 정의함

```
services:
  mysql:
    image: alicek106/composetest:mysql
    links:
      - db:database
    environment:
      - MYSQL_ROOT_PASSWORD=mypassword
    command: apachectl -DFOREGROUND
    depends_on:
      - something
    ports:
      - "80:80"
```

- `image`: 서비스의 컨테이너를 생성할 때 쓰일 이미지의 이름을 설정함
- `links`: 서비스 컨테이너 내에서 다른 서비스에 서비스명으로 접근하거나 `service:alias`의 형식을 사용하면 서비스 내에서 별칭으로도 접근할 수 있도록 설정함
- `environment`: 서비스 컨테이너 내에서 사용할 환경변수를 지정함
- `command`: 컨테이너가 실행될 때 수행할 명령어를 설정함
- `depends_on`: 특정 컨테이너에 대한 의존 관계를 나타내며, 여기에 정의된 컨테이너가 먼저 생성되고 해당 서비스 컨테이너가 생성됨
- `ports`: 서비스의 컨테이너를 개방할 포트를 설정하며, 만약 `80:80`과 같이 설정하는 경우 `docker compose scale` 명령어로 컨테이너 수를 늘릴 수 없음

### 네트워크 정의

```
services:
  myService:
    image: nginx
    networks:
      - myNetwork
networks:
  myNetwork:
    driver: overlay
  ipam:
    driver: mydriver
    config:
      subnet: 172.20.0.0/16
      ip_range: 172.20.5.0/24
      gateway: 172.20.5.1
```

- `driver`: 도커 컴포즈는 생성된 컨테이너를 위한 기본적으로 브리지 타입의 네트워크를 생성하지만, YAML 파일에서 `driver` 항목을 정의해 다른 네트워크를 사용하도록 설정할 수 있음
- `ipam`: IAM(IP Adress Manager)를 위해 사용할 수 있는 옵션으로서 `subnet`, `ip` 범위 등을 설정할 수 있음

```
services:
  myService:
    image: nginx
    networks:
      - myNetwork
networks:
  myNetwork:
    external: true  
```

- `external`: 프로젝트를 생성할 때마다 네트워크를 생성하는 것이 아닌, 기존의 네트워크를 사용하도록 설정함. 사전에 존재하는 네트워크를 사용하므로 `driver`, `ipam` 옵션과 함께 사용할 수 없음

### 볼륨 정의

```
services:
  mysql:
    volumes:
      - my_test_data:/data
volumes:
  my_test_data:
    driver: local
```

- `driver`: 볼륨을 생성할 때 사용될 드라이버를 설정하며, 기본값은 `local`임