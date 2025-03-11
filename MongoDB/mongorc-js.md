# .mongorc.js

```js
// .mongorc.js
var compliment = ["attractive", "intelligent", "like Batman"];
var index = Math.floor(Math.rnadom() * 3);

print("Hello, you're looking particularly " + compliment[index] + " today!");
```

```
$ mongo
MongoDB shell version: 4.2.1
connecting to: test
Hello, you're looking particularly like Batman today!
```

* 홈 디렉토리에 `.mongorc.js`가 있으면, `mongo` 명령으로 셸을 시작할 때 자동으로 스크립트가 실행됨
* 셸 스크립트 안에서는 아래의 경우를 제외하곤 대부분 셸에서 db를 사용하는 것과 동일하게 사용할 수 있음
	* `use '데이터베이스명'` -> `db.getSisterDB("데이터베이스명")`
	*  `show dbs` -> `db.getMongo().getDbs()`
	* `show collections` -> `db.getCollectionNames()`
* 이 스크립트를 이용해 사용하고 싶은 전역 변수를 설정하거나, 긴 별칭을 짧게 하고, 내장 함수를 재정의함

```js
var notAllowed = function() { print("Not allowed command"); };

// 데이터베이스 삭제 방지
db.dropDatabase = DB.prototype.dropDatabase = notAllowed;

// 컬렉션 삭제 방지
DBCollection.prototype.drop = notAllowed;

// 인덱스 삭제 방지
DBCollection.prototype.dropIndex = notAllowed;
DBCollection.prototype.dropIndexes = notAllowed;
```

* 위의 경우는 `.mongorc.js`의 일반적인 용법으로, 위험한 셸 명령을 수행할 시에 오류 메시지가 출력됨
* 이는 프로그래머가 의도치 않게 키를 잘못 눌러 잘못된 명령을 내리는 것을 방지함
* 셸을 시작할 때 `--norc` 옵션을 이용하면, 별도로 `.mongorc.js` 로딩을 하지 않고 시작됨