# GridFS

* 몽고DB에서 대용량 이진 파일을 저장하는 매커니즘으로, 몽고DB 내에 파일을 저장할 때 사용됨
* 몽고DB를 사용 중이라면 파일 스토리지를 위한 별도의 도구 대신 이용 가능하므로, 기술 스택이 단순화됨
* GridFS는 몽고DB의 복제나 샤딩을 이용할 수 있어, 파일 스토리지를 위한 장애 조치와 분산 확장이 쉬움

### GridFS 시작하기

```shell
$ mongofiles put foo.txt
2019-10-30T10:12:06.588+0000 connected to: localhost
2019-10-30T10:12:06.588+0000 added file: foo.txt
$ mongofiles list
2019-10-30T10:12:41.603+0000 connected to: localhost
foo.txt 13
$ mongofiles get foo.txt
2019-10-30T10:13:23.948+0000 connected to: localhost
2019-10-30T10:13:23.955+0000 finished writing to foo.txt
```

* 몽고DB 설치 시 기본적으로 제공되는 mongofiles 유틸리티로 GridFS를 쉽게 설치하고 운영할 수 있음
* mongofiles에서 `put` 연산은 파일시스템으로부터 파일을 받아 GridFS에 추가함
* `list` 연산은 GridFS에 올린 파일 목록을 보여주고, `get`은 GridFS에서 파일을 받는 역할임