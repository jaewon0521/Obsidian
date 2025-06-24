
Server Component

하이드레이션이 필요 없는 컴포넌트는 서버에서 실행한다.

React Dom을 서버에서 HTML 파일로 변환하여 서버에서 응답 해준다.

그 이후 JS Bundle 파일을 응답 한다. 
JS Bundle 파일은 Dom 상호작용, window 접근 코드이다. 

하지만 JS Bundle 파일을 실행하지 않고 파일만 응답 해주는데 왜 Server Component에서는 오류가 발생하며 use client에서 실행이 되어야 할까

또 한, 리액트 훅이 포함되어 있는 코드는 Server에서 실행되어 지는데 왜 리액트 훅이 포함되어 있는 코드는 왜 use Client로 분류 해야 할까.

Client Component

use client
해당 파일을 client 컴포넌트로 지정하는 지시자 이다.

use client를 선언해도 해당 파일이 CSR은 아니다.

use server
해당 파일을 Server Component로 지정하는 지시자 이다.

use server를 선언해도 해당 파일이 SSR은 아니다.

SSR
Server Component와 SSR은 엄연히 다르다.

CSR
Client Component와  CSR은 엄연히 다르다.


라고 알고 있는데 왜 이렇게 되는지 이해가 안돼.