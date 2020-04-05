# mkdir 하고 cd 한번에 하기
2020-04-02
</hr>
TIL을 시작해보려고 하는데 아래 두 커맨드를 한 번에 하고 싶어졌다.

```console
$ mkdir Linux
$ cd Linux
```

이렇게 하면 될 거 같다.

```console
$ mkdir Linux && cd Linux
```

`&&` 의 의미
- logical and
- 전 command가 true (성공)이면 `&&` 다음 것도 실행한다.

### 구글링 하면 나오는 방법
함수로 저장해서 사용

```bash
mkcdir ()
{
    mkdir -p -- "$1" &&
      cd -P -- "$1"
}
```

`$_` 사용 (직전 커멘드의 마지막 argument를 가지고 있는다.)
- `""`로 안 감싸주면 argument에 공백이 있는 경우 안된다고 하는데 나는 잘된다.

```console
$ mkdir foo && cd "$_"
```

  
