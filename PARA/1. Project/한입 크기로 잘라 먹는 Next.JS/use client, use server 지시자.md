
Server Component

하이드레이션이 필요 없는 컴포넌트는 서버에서 실행한다.

React Dom을 서버에서 HTML 파일로 변환하여 서버에서 응답 해준다.

그 이후 JS Bundle 파일을 응답 한다. 
JS Bundle 파일은 Dom 상호작용, window 접근 코드이다. 
하지만 JS Bundle 파일을 실행하지 않고 파일만 응답 해주는데 ㅇclient에서 실행이 되어야 한다.
리액트 훅이 포함되어 있는 코드는 Server에서 실행되어 진다.

하지만 리액트 훅이 포함되어 있는 코드는 왜 use Client로 분류 해야 할까.

Client Component

use client

use server

SSR

CSR