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
READ UNCOMMITTED은 이름에서 알 수 있듯이 commit되지 않은 row를 읽을 수 있다. 
``` SQL

``` 

#### READ COMMITTED

#### REPEATABLE READ

#### SERIALIZABLE