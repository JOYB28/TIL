# 객체지향?
20210207 (일)

객체 지향 프로그래밍에서 역할과 구현을 분리함으로써 역할들의 협력관계를 변경없이 재사용할 수 있다.

예를 들어, `MemberService`는 회원을 저장할 때 `MemberRepository`의 구현체의 내부 구현을 알 필요없이 인터페이스에 정의된 함수를 사용하면 된다. 

대충 코드를 짜보면 아래와 같다.

``` java
// 편의상 MemberService는 인터페이스 X
public class MemberService {

    private MemberRepository memberRepository = new JdbcMemberRepository();
    // private MemberRepository memberRepository = new JpaMemberRepository();

}

public interface MemberRepository {
    ...
}

public class JdbcMemberRepositry implements MemberRepository {
    ...
}

public class JpaMemberRepositry implements MemberRepository {
    ...
}

```

그런데 코드에서 볼 수 있듯이 `MemberService`는 인터페이스인 `MemberRepository`에 의존하는게 아니라 각 구현체에 의존하고 있다. 즉 이 형태에서는 객체지향 원리중 하나인 OCP(Open Close Principle)를 만족하지 못한다. 변경에 닫혀있지 않기 떄문이다. (실제 구현체를 선언하는 부분을 고쳐야 한다.)

