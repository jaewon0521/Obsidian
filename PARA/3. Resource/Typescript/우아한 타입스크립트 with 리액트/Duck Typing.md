#### 개요
- 다른 강타입 언어는 이름으로 타입을 구분한다.
- 타입스크립트는 덕타이핑 기반으로 한다.

#### 의미
어떤 타입에 부합하는 변수와 메서드를 가질 경우 해당 타입에 속하는 것으로 간주하는 방식이다.
`
JAVA

class Cat {
	String name;
}

class Arrow {
	String name;
}

public class Main {
...
	Arrow cat = new Cat(); // error : incompatible types
	Cat arrow = new Arrow(); // error incompatible types
}
`