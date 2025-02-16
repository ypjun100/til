# Git Checkout

* `reset` 명령과 마찬가지로 `checkout` 명령도 `HEAD`, `Index`, `Working Area`의 세 트리를 조작함
* 하지만 `checkout` 명령의 경우 파일 경로를 함께 사용하는지에 따라 작동되는 동작의 차이가 존재함

### 경로가 없는 경우

```shell
$ git checkout [branch]
```

* 위 명령은 `git reset --hard [branch]` 명령과 비슷하게 특정 브랜치를 기준으로 세 트리를 조작함
* 하지만 `checkout` 명령은 저장하지 않은 것이 있는지 확인하기 때문에 워킹 디렉토리를 안전하게 다룸
	* 반면에 `reset --hard` 명령은 `checkout`과 달리 이를 확인하지 않고 모든 것을 변경함
* 또한, `reset`은 `HEAD`가 가리키는 브랜치를 움직이지만, `checkout`은 `HEAD` 자체를 다른 브랜치로 옮김
* 즉, `reset`은 `HEAD`가 가리키는 브랜치의 포인터를 옮기고, `checkout` 명령은 `HEAD` 자체를 옮김

### 경로가 있는 경우

```shell
$ git checkout [branch] [file]
```

* 경로를 지정한 `checkout` 명령은 인덱스의 내용 뿐 아니라 워킹 디렉토리의 파일도 해당 커밋으로 변경함
* `git rest --hard [branch] file` 명령과 동일하게 작동하며 안전하지 않고 HEAD도 움직이지 않음

|                          | HEAD | Index | Workdir | WD Safe |
| ------------------------ | ---- | ----- | ------- | ------- |
| **Commit Level**         |      |       |         |         |
| reset --soft [commit]    | REF  | NO    | NO      | YES     |
| reset [commit]           | REF  | YES   | NO      | YES     |
| reset --hard [commit]    | REF  | YES   | YES     | NO      |
| checkout [commit]        | HEAD | YES   | YES     | YES     |
| **File Level**           |      |       |         |         |
| reset (commit) [file]    | NO   | YES   | NO      | YES     |
| checkout (commit) [file] | NO   | YES   | YES     | NO      |

* HEAD가 'REF'라면 HEAD가 가리키는 브랜치를 움직이고, 'HEAD'이면 HEAD 자체가 움직인다는 뜻
* WD Safe 열이 'NO'라면 워킹 디렉토리에 저장하지 않은 내용들이 안전하지 않기 때문에 조심해야 함