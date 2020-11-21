# 2주차: 자바 데이터 타입, 변수 그리고 배열

백기선님이 여신 [스터디 할래](https://github.com/whiteship/live-study)에 참여합니다.

## 프리미티브 타입 종류와 값의 범위 그리고 기본 값
프리미티브 타입은 언어에 의해 사전 정의되어 있으며 예약된 키워드를 사용한다. 기본값이 항상 있어서 Null이 존재하지 않는다. 값이 스택 메모리에 저장된다.
- **byte**: 8-bit signed [Two's complement](https://en.wikipedia.org/wiki/Two%27s_complement) integer. 값의 범위는 -128 ~ 127으로 총 256개를 표현한다. 기본값은 0.
- **short**: 16-bit signed two's complement interger. 값의 범위는 -32,768 ~ 32767. 기본값은 0.
- **int**: 32-bit signed two's complement integer. 값의 범위는 -2^31 ~ 2^31-1. 기본값은 0.
- **long**: 64-bit signed two;s complement integer. 값의 범위는 -2^63 ~ 2^63-1. 기본값은 0L. Java SE 8 이후에는 `long` 타입을 unsigned 64-bit long으로 사용할 수도 있다. 이 때 값의 범위는 0 ~ 2^64-1
- **float**: [single-precision](https://en.wikipedia.org/wiki/Single-precision_floating-point_format) 32-bit IEEE 755 floating point. 완벽하게 정확하지 않기 때문에 정확한 계산을 요하는 경우(돈)에는 `float`을 사용하면 안되고 `java.math.BigDecimal`을 사용해야 한다. 기본값은 `0.0F`.
- **double**: [double-precision](https://en.wikipedia.org/wiki/Double-precision_floating-point_format) 64-bit IEEE 754 floating point. 기본값은 `0.0d`.
- **boolean**: `true`, `false` 값중 하나를 가짐. 기본값은 `false`.
- **char**: single 16-bit Unicode charater. 최소값은 `'\u0000'`, 최대값은 `'\uffff'`이며 기본값은 `'\u0000'`.
- 추가: **string**은 프리미티브 타입은 아니지만 유사하게 고려된다.

## 프리미티브 타입과 레퍼런스 타입
실제 값을 스택 메모리에 저장하는 프리미티브 타입과 다르게 래퍼런스 타입은 이름 뜻 그대로 값이 저장되는 주소값을 저장하고 실제 값은 힙 메모리에 저장된다.

프리미티브 타입을 제외하고 나머지는 모두 래퍼런스 타입이다.
기본값이 null이다. 

예시로는 배열(Array), 열거(Enumeration), 클래스(Class) 등이 있다.

## 리터럴
프리미티브 타입의 변수를 초기화할 때는 `new` 키워드를 사용하지 않는다. 이는 프리미티브 타입이 언어에 내장된 특수한 데이터 타입이기 때문이며 `class`로부터 생성되는 `object`가 아니다. 리터럴은 고정된 값을 표현하는 source code이다. 리터럴은 연산 없이 바로 표현할 수 있다.
``` java
    // 예시
    boolean result = true;
    char c = 'A';
    byte b = 100;
    short s = 100000;
    // int type integer 리터럴
    int i = 1000000; 
    // long type integer 리터럴
    long l = 1L; 
    int hexVal = 0x1a; // hexadecimal
    int binVal = 0b11010; // binary
    // double type floating-point 리터럴
    double d1 = 123.4;
    double d2 = 1.234e2; // d1과 동일
    // float type floating-point 리터럴
    float f1 = 123.4f;
    String s = "joyb"
```

## 변수 선언 및 초기화하는 방법
``` java
    int i; // int 타입의 i라는 변수 선언
    i = 100; // 값 초기화

    int c = 100; // 변수 선언과 초기화를 한번에

    int a, b;
    int x = 10, y = 20, z = 30;
```

## 변수의 스코프와 라이프타임


## 타입 변환, 캐스팅 그리고 타입 프로모션

## 1차 및 2차 배열 선언하기

## 타입 추론, var

### 참고
- https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html
- https://gbsb.tistory.com/6
