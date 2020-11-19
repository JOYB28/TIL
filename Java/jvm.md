# 1주차: JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가
백기선님이 여신 [스터디 할래](https://github.com/whiteship/live-study/issues/1)에 참여합니다.

## JVM이란 무엇인가
- 자바 가상 머신 (Java Virtual Machine)
- Java 애플리케이션을 실행하기 위한 런타임 환경을 제공하는 엔진
- Java 바이트코드를 기계어로 변환
- Java Run Environment(JRE)의 한 부분
- Write Once Run Everywhere

## 컴파일 하는 방법
- Java 프로그램 컴파일은 보통 `.java` 파일을 `.class` 파일로 변환하는 것을 말한다.
- 컴파일로는 javac를 사용할 수 있다.
``` java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```
``` shell
$ javac HelloWorld.java
$ ls
HelloWorld.class HelloWorld.java
```
## 실행하는 방법
``` shell
$ java HelloWorld
Hello World!
```
## 바이트코드란 무엇인가
## JIT 컴파일러란 무엇이며 어떻게 동작하는지
## JVM 구성 요소
## JDK와 JRE의 차이



