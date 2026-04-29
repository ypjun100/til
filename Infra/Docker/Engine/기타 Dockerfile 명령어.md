# 기타 Dockerfile 명령어

### ENV

```Dockerfile
FROM ubuntu:24.04
ENV test=/home
WORKDIR $test
RUN touch $test/mytouchfile
```

- `Dockerfile`에서 사용될 환경변수를 지정하며, 설정한 환경변수는 `$ENV_NAME`으로 사용할 수 있음
- 환경변수는 `Dockerfile` 내에서 뿐 아니라 이미지 컨테이너 내에서도 사용할 수 있음
	- `docker run` 명령어의 `-e` 옵션을 사용하면 기존에 설정된 변수는 덮어씌어짐

### VOLUME

```Dockerfile
FROM ubuntu:24.04
RUN mkdir /home/volume
RUN echo test >> /home/volume/testfile
VOLUME /home/volume
```

- 컨테이너 내부의 특정 디렉토리를 호스트 또는 다른 컨테이너와 공유하도록 마운트 지점을 선언함
- 컨테이너 실행 시 `VOLUME`에 선언된 경로는 익명 볼륨으로 생성되며, 호스트 도커 내부 폴더에 연결됨

> [!Question] `VOLUME`과 `docker run -v`의 차이점
> `Dockerfile`에 `VOLUME`을 명시한 컨테이너 실행 시에 자동으로 익명 볼륨이 생성되긴 하지만, 생성된 볼륨을 제대로 사용하기 위해서는 `docker run -v` 옵션을 사용하여 Named Volume을 구성하거나 Bind Mount를 설정해야 한다. 그렇다면 `VOLUME`을 왜 사용해야 하는가 하면, 먼저 개발자가 실수로 `docker run` 시에 `-v`를 붙이지 않아도 익명 볼륨을 생성하기에 데이터 유실 방지가 되며, 이미지 사용자에게 의도를 명시적으로 전달하는 문서화 역할을 한다.

### ARG

```Dockerfile
FROM ubuntu:24.04
ARG my_arg
ARG my_arg_2=valeu2
RUN touch ${my_arg}/mytouch
```

- `build` 명령어를 실행할 때 추가로 입력을 받아 `Dockerfile` 내에서 사용될 변수의 값을 설정함
- 위는 `my_arg`와 `my_arg2`라는 변수를 입력받을 것이라 명시하며, `my_arg2`처럼 기본값 설정이 가능함
- `docker build` 명령어 실행 시에 `--build-arg` 옵션을 사용해 `ARG`에 값을 입력할 수 있음
	- ex) `docker build --build-arg my_arg=/home -t myarg:0.0 .`

### USER

```Dockerfile
RUN groupadd -r group1 && useradd -r -g group1 jun
USER jun
```

- `USER`로 컨테이너 내에서 사용될 사용자 계정을 설정하면, 그 아래의 명령어는 해당 사용자 권한으로 실행됨
- 보통 루트 권한은 지양하기에 `RUN`으로 사용자의 그룹과 계정을 생성한 뒤에 `USER`를 사용함

### HEALTHCHECK

```Dockerfile
FROM nginx
RUN apt-get update -y && apt-get install curl -y
HEALTHCHECK --interval=1m --timeout=3s --retries=3 CMD curl -f http://localhost || exit 1
```

- 이미지로부터 생성된 컨테이너에서 동작하는 애플리케이션의 상태를 체크하도록 설정함
- 컨테이너 내의 애플리케이션 프로세스가 종료되진 않았으나, 동작하고 있지 않은 상태를 방지할 수 있음
- 위 예시는 1분마다 `curl`을 실행하며, 3초 이상의 실패 3번이 발생하면 컨테이너는 `unhealthy`가 됨
- 이미지 빌드 및 컨테이너를 생성하면 `docker ps` 출력에 컨테이너의 `STATUS`에 상태 정보가 추가됨

> [!TIP]
> `HEALTHCHECK` 명령은 단순히 컨테이너에 대한 상태 체크만 할뿐 그 이후의 액션을 따로 하진 않는다. 따라서 Unhealthy 상태의 경우에 자동 재시작을 한다던지의 기능은 기본적으로 제공하지 않고 K8S를 사용하여 `livenessProbe` 기능을 사용하는 등의 차선책이 필요하다.

