# Dockerfile

- 도커는 이미지를 생성하기 위해 필요한 패키지, 코드를 쉽게 기록하고 수행할 수 있는 빌드 명령어를 제공함
- 필요한 리소스를 기록해 두면 도커는 이 파일을 읽어 컨테이너에서 작업을 수행한 뒤 이미지로 만들어냄
- 기록한 파일의 이름을 `Dockerfile`이라 하며, 빌드 명령은 `Dockerfile`을 읽어 이미지를 생성함

### Dockerfile 작성

```dockerfile
FROM node:20-slim

LABEL maintainer="yourname@example.com" \
      version="1.0.0" \
      description="Node.js Express App"

RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    && rm -rf /var/lib/apt/lists/*

ADD package*.json /app/

WORKDIR /app

RUN npm install

ADD . /app/

EXPOSE 3000

CMD ["node", "server.js"]
```

- **`FROM`** : 생성할 이미지의 베이스가 될 이미지로, `Dockerfile`을 작성할 때 한 번 이상 입력해야 함
- **`LABEL`** : 이미지에 '키:값' 형태의 메타데이터를 추가하며, 이 데이터는 `docker inspect`로 확인 가능함
- **`RUN`** : 이미지를 만들기 위해 컨테이너 내부에서 명령어를 실행함 (명령 중에 별도의 입력을 받을 순 없음)
- **`ADD`** : 파일에 이미지를 추가하며, 추가하는 파일은 `Dockerfile`이 위치한 컨텍스트 디렉터리임
- **`WORKDIR`** : 명령어를 실행할 디렉터리를 명시하며, Bash에서 `cd` 명령어를 입력하는 것과 같은 기능임
- **`EXPOSE`** : `Dockerfile`의 빌드로 생성된 이미지에서 노출할 포트를 설정함 (실제 포트 바인딩은 안 됨)
	- 단지 컨테이너의 80번 포트를 사용할 것임을 나타낼 뿐이며, 실제 호스트의 포트와 바인딩은 안 됨
- **`CMD`** : 컨테이너가 시작될 때 실행할 명령어를 설정하며, `Dockerfile`에서 한 번만 사용할 수 있음
	- `docker run` 명령어에서 커맨드 인자를 입력하면, `CMD`의 명령어는 `run` 인자로 덮어 씌어짐

### 이미지 생성

```shell
docker build -t mybuild:0.0 ./
```

- `Dockerfile`에서 정의한 이미지 생성을 위해 빌드를 진행하려면 위와 같은 명령어를 사용함
- `-t` 옵션은 생성될 이미지의 이름을 설정하며, 위 명령에서는 `mybuild:0.0`이라는 이미지로 생성됨

```shell
docker run -d -P --name myserver mybuild:0.0
```

- `-P`(`--publish-all`) 옵션은 이미지에 설정된 `EXPOSE`의 모든 포트를 호스트에 연결하도록 설정함