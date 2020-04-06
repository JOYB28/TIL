# 트랜잭션의 isolation level
2020-04-06
</hr>
분명 DB 수업 때 이에 대해 배웠겠지만, 현업에서 개발하기 전까지 까먹고 있던 트랜잭션의 isolation level. 이제는 정말 알고 넘어갈 때가 되어 정리한다.

### 트랜잭션이란
데이터베이스의 맥락에서 트랜잭션이란 데이터에 독립적으로 수행되는 논리적 단위 (logical unit)이다. 흔히 트랜잭션은 ACID 성질을 가진다고 말한다.
- Atomicity (원자성)
- Consistency (일관성)
- Isolation (고립성)
- Durability (지속성)

### 트랜잭션 isolation level
트랜잭션이 여러 개 있을 때 어떻게 수행할 지를 정하는 것?

#### 종류
- READ UNCOMMITTED
- READ COMMITTED
- REPEATABLE READ
- SERIALIZABLE 

#### READ UNCOMMITTED
READ UNCOMMITTED은 이름에서 알 수 있듯이 `commit`되지 않은 row를 읽을 수 있다. 따라서 읽었던 정보가 `rollback`될 때 `dirty read`문제가 발생한다.

아래와 같은 상태가 있다고 하자.
```sql
mysql> select * from book;
+----+----------------------+
| id | name                 |
+----+----------------------+
|  1 | Beauty and the Beast |
|  2 | La La Land           |
|  3 | Sherlock Holmes      |
+----+----------------------+
3 rows in set (0.00 sec)
```

READ UNCOMMITTED로 isolation level을 설정하고 transaction1을 시작한다.

##### transaction1
```sql
mysql> set session transaction isolation level READ UNCOMMITTED;
Query OK, 0 rows affected (0.00 sec)

mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from book;
+----+----------------------+
| id | name                 |
+----+----------------------+
|  1 | Beauty and the Beast |
|  2 | La La Land           |
|  3 | Sherlock Holmes      |
+----+----------------------+
3 rows in set (0.00 sec)
```

다른 터미널에서 transaction2를 만들어 id가 2인 book의 name을 바꿔보자.

##### transaction2
```sql
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> update book
    -> set name = "La La La Land"
    -> where id = 2;
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

이 후 transaction1에서 다시 select를 해보면, `COMMIT`되지 않은 결과가 읽어짐을 볼 수 있다. 이를 `dirty read`라고 한다. 만약 transaction2가 `ROLLBACK`되면 "La La La Land"라는 이름을 가진 book은 없는데 읽은 결과에 남게 된다.

##### transaction1
```sql
mysql> select * from book;
+----+----------------------+
| id | name                 |
+----+----------------------+
|  1 | Beauty and the Beast |
|  2 | La La La Land        |
|  3 | Sherlock Holmes      |
+----+----------------------+
3 rows in set (0.00 sec)
```

#### READ COMMITTED
READ COMMITTED에서는 `COMMIT`된 row만 읽게 되어 READ UNCOMMITTED에서 발생하는 `dirty read`를 방지한다. 하지만 `repeatable read` 문제가 있다.
위 상황에서 transaction2가 `COMMIT`되면 여전히 한 트랜잭션 내에서 읽은 값이 다른 문제가 발생한다. (`repeatable read`)

##### transaction1
```sql
mysql> set session transaction isolation level READ COMMITTED;
Query OK, 0 rows affected (0.00 sec)

mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from book;
+----+----------------------+
| id | name                 |
+----+----------------------+
|  1 | Beauty and the Beast |
|  2 | La La Land           |
|  3 | Sherlock Holmes      |
+----+----------------------+
3 rows in set (0.00 sec)

mysql> select * from book; -- transaction2가 COMMIT 된 후
+----+----------------------+
| id | name                 |
+----+----------------------+
|  1 | Beauty and the Beast |
|  2 | La La La Land        |
|  3 | Sherlock Holmes      |
+----+----------------------+
3 rows in set (0.00 sec)
```

#### REPEATABLE READ
isolation level을 `REPEATABLE READ`로 두고 위 작업을 해보자.

##### transaction2
```sql
mysql> update book
    -> set name = "La La La La Land"
    -> where id = 2;
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

##### transaction1
```sql
mysql> select * from book; -- update 전
+----+----------------------+
| id | name                 |
+----+----------------------+
|  1 | Beauty and the Beast |
|  2 | La La La Land        |
|  3 | Sherlock Holmes      |
+----+----------------------+
3 rows in set (0.00 sec)

mysql> select * from book; -- update 후
+----+----------------------+
| id | name                 |
+----+----------------------+
|  1 | Beauty and the Beast |
|  2 | La La La Land        |
|  3 | Sherlock Holmes      |
+----+----------------------+
3 rows in set (0.00 sec)
```

영향을 받지 않았다.
그렇다면 transaction3에서 새로운 row를 삽입하면 어떻게 될까?

##### transaction3
```sql
mysql> insert into book (name) values ("Harry Potter");
Query OK, 1 row affected (0.00 sec)

mysql> select * from book;
+----+----------------------+
| id | name                 |
+----+----------------------+
|  1 | Beauty and the Beast |
|  2 | La La La Land        |
|  3 | Sherlock Holmes      |
|  4 | Harry Potter         |
+----+----------------------+
4 rows in set (0.00 sec)
```

##### transaction1
```sql
mysql> select * from book; -- transaction3 후
+----+----------------------+
| id | name                 |
+----+----------------------+
|  1 | Beauty and the Beast |
|  2 | La La La Land        |
|  3 | Sherlock Holmes      |
+----+----------------------+
```

영향을 받지 않았다. 사실 transaction1에서 id가 4인 row가 생겨야 `phantom read`가 발생한 것이고, 그렇기에 `SERIALIZABLE`을 설명해야 하는데 벌써 해결이 되었다.
이는 mysql이 snapshot based로 구현되기 때문이라고 하는데 정확히는 모르겠다. 

#### SERIALIZABLE
이 단계에서는 읽는 row에 `shared lock`이 걸리기 때문에 `dirty read`, `repeatable read`, `phantom read`가 모두 생기지 않는다? 좀 더 공부가 필요하다.. 

### 정리

| isolation level | dirty read | repeatable read | phantom read |
|---|:---:|:---:|:---:|
| `READ UNCOMMITTED` | O | O | O |
| `READ COMMITTED` | X | O | O |
| `REPEATABLE READ` | X | X | O (mysql에서 X) |
| `SERIALIZABLE` | X | X | X |

isolation level이 높아지면, 트랜잭션의 동시성이 떨어지기 때문에 성능이 떨어진다. isolation level을 상황에 맞게 조절하여 사용해야 한다.

### 회고
완벽한 정리를 하지 못했지만 의미가 있겠지..?

### 참고
- https://jupiny.com/2018/11/30/mysql-transaction-isolation-levels/ 
    - 많은 도움을 받았다.

