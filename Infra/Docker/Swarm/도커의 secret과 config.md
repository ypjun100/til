# 도커의 secret과 config

- 외부에서 컨테이너 안으로 데이터를 주입하는 Docker Swarm 기능으로, `docker run`에서는 사용 불가함

### Secret

```shell
# secret 생성 및 조회
echo 1q2w3e4r | docker secret create my_password -
docker secret inspect my_password # 실제 값은 보여지지 않음

# 서비스에 적용
docker service create \
--name mysql \
--secret source=my_password,target=mysql_root_password \
--secret source=my_password,target=mysql_password \
-e MYSQL_ROOT_PASSWORD_FILE="/run/secrets/mysql_root_password" \
-e MYSQL_PASSWORD_FILE="/run/secrets/mysql_password" \
-e MYSQL_DATABASE="wordpress" \
mysql
```

- 비밀번호, SSH 키, API 키, TLS 인증서 등 보안에 민감한 데이터를 저장하기 위한 기능임
- secret 값은 매니저 노드 간에 암호화된 상태로 저장되며, 노드 간 전송 시에는 mTLS를 이용함
- 컨테이너가 중단되면 메모리에서 즉시 삭제되며, `inspect` 시에 해당 데이터가 노출되지 않음
- 컨테이너로 공유된 secret 값은 기본적으로 컨테이너 내부의 `/run/secrets` 디렉토리에 마운트됨

### Config

- 암호화가 필요 없는 설정값(nginx 설정, yml 파일 등)을 저장하는 데 사용하는 기능임
- 스웜 클러스터의 내부 DB에 저장되며, 서비스 배포 시 컨테이너 내 특정 경로에 파일로 마운트됨
- `inpect` 시 설정한 값이 노출되며, base64 디코딩으로 원문을 확인할 수 있음