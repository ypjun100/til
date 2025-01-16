# REST API의 구성 및 설계 원칙

| 구성 요소               | 내용                | 표현 방법       |
| ------------------- | ----------------- | ----------- |
| 자원(resource)        | 자원                | URI(엔드포인트)  |
| 행위(verb)            | 자원에 대한 행위         | HTTP 요청 메서드 |
| 표현(repersentations) | 자원에 대한 행위의 구체적 내용 | 페이로드        |

* REST API는 자원(resource), 행위(verb), 표현(representations)의 3가지 요소로 구성됨
* 자체 표현 구조로 구성되어 REST API만으로 요청의 내용을 이해할 수 있음
* URI는 리소스를 표현하는 데 집중하고, HTTP 요청 메서드는 행위에 대한 정의에 집중해야 함

### 1. URI는 리소스를 표현해야 함

```
# bad
GET /getTodos/1
GET /todos/show/1

# good
GET /todos/1
```

* URI는 리소스를 표현하는 데 중점을 두어야 하며, 식별을 위한 이름으로 동사보다는 명사를 사용함
	* 따라서, 이름에 `get`과 같은 행위에 대한 표현이 들어가서는 안됨

### 2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현함

```
# bad
GET /todos/delete/1

# good
DELETE /todos/1
```

* HTTP 요청 메서드는 서버에게 요청의 종류와 목적을 알리는 방법으로 5가지의 메서드를 사용하여 구현함

| HTTP 요청 메서드 | 종류             | 목적           | 페이로드 |
| ----------- | -------------- | ------------ | ---- |
| GET         | index/retrieve | 모든/특정 리소스 취득 | X    |
| POST        | create         | 리소스 생성       | O    |
| PUT         | replace        | 리소스의 전체 교체   | O    |
| PATCH       | modify         | 리소스의 일부 수정   | O    |
| DELETE      | delete         | 모든/특정 리소스 삭제 | X    |
