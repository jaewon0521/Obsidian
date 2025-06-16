
#### 개요
RSC Payload 
JS Build Load

#### 주의 사항
1. 서버 컴포넌트에는 브라우저에서 실행될 코드가 포함되면 안된다.
2. 클라이언트 컴포넌트는 클라이언트에서만 실행되지 않는다.
3. 클라이언트 컴포넌트에서 서버 컴포넌트를 import 할 수 없다.
4. 서버 컴포넌트에서 클라이언트 컴포넌트에게 직렬화 되지 않는 Props는 전달 불가하다.


SSG RSC Payload + JS Build Load

SSR RSC Payload
- Dynamic Param

env 환경 변수 설정

접두사로 NEXT_PUBLIC 을 상요하면 Client, Server 둘다 접근 할 수 있다
접두사를 생략하게 된다면 Server에서만 접근 할 수 있다.
