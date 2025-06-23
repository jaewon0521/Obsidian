특정 경로에서 발생한 모든 오류들을 한꺼번에 처리할 수 있는 에러 핸들링 기능이다.


error.tsx 파일은 반드시 'use client' 지시자를 선언해야 한다.

에러는 가장 가까운 부모 에러 경계부터 자식에게 계층 구조로 전파 한다.

Error 컴포넌트는 error, reset Props를 전달 받으며

error는 Javascript의 Error 객체이며, reset은 오류 객체를 초기화 하는 함수를 받ㄴ