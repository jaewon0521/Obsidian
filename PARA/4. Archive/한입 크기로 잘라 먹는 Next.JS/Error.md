#### 개요 
특정 경로에서 발생한 모든 오류들을 한꺼번에 처리할 수 있는 에러 핸들링 기능이다.

Next.js의 App Router는 `error.tsx` 파일을 통해 해당 경로에서 발생한 런타임 오류를 포착하고 사용자에게 대체 UI를 제공할 수 있다.

---

#### 주의사항 
error.tsx 파일은 반드시 `'use client'` 지시자를 선언해야 한다.

에러 경계(Error Boundary)는 React의 클라이언트 컴포넌트 기능으로 동작하기 때문에, `error.tsx` 파일에는 반드시 `'use client'` 지시자가 필요하다. 

이를 생략하면 서버 컴포넌트로 인식되어 에러 경계로서 동작하지 않는다.

---
#### 특징
에러는 가장 가까운 부모 에러 경계부터 자식에게 계층 구조로 전파한다.

Next.js는 각 라우트 계층별로 `error.tsx` 파일을 둘 수 있으며, 하위 경로에서 오류가 발생하면 가장 가까운 상위의 `error.tsx`가 실행된다.

이는 React의 에러 버블링(Error Bubbling) 개념과 동일하며, 경로 기반의 에러 캡처가 가능하다.

---

#### Props
Error 컴포넌트는 `error`, `reset` Props를 전달 받는다.

- `error`: JavaScript의 `Error` 객체로, 어떤 에러가 발생했는지를 담고 있다.
- `reset`: 에러 상태를 초기화하는 함수로, 호출 시 현재 에러를 리셋하고 정상 상태로 되돌린다.

이 두 개의 Props를 통해 사용자에게 에러 메시지를 표시하거나, 재시도 버튼 등을 구현할 수 있다.
##### reset
reset() 함수는 에러를 초기화하지만 페이지를 다시 로드하지는 않는다.

`reset()` 함수는 내부적으로 에러 상태를 초기화할 뿐, 서버 컴포넌트를 새로 요청하거나 페이지를 리로드하지는 않는다.  
따라서 **서버 요청을 다시 시도하거나 데이터를 다시 불러오고 싶다면 추가적인 동작이 필요**하다.

#### 페이지를 다시 요청하는 방법
#### ❌ `window.location.reload()` 사용
- 브라우저의 전체 페이지를 새로고침하므로 Next.js의 서버 컴포넌트, 캐시 등의 장점을 활용하지 못한다.
#### ✅ `router.refresh()` 사용
- `useRouter()` 훅을 통해 `router.refresh()` 메서드를 호출하면, **현재 경로의 서버 컴포넌트 데이터를 다시 가져온다.**
- 이 방식은 Next.js의 서버 렌더링 캐시 및 Streaming 기능을 활용할 수 있어 성능적으로 우수하다.
---
#### router.refresh() 호출 시 주의할 점

`router.refresh()`는 내부적으로 비동기이지만, 함수 자체는 Promise를 반환하지 않기 때문에 `await`를 사용할 수 없다. 

따라서 다음과 같은 방식으로 동기적인 흐름에서 호출하는 것이 안전하다.
#### ✅ `startTransition()`을 사용한 리프레시 처리
``` tsx
import { useRouter } from "next/navigation";
import { startTransition } from "react";

const router = useRouter();

startTransition(() => {
  router.refresh();
  reset(); // 에러 상태 초기화
});

```
- `startTransition()`은 React의 병렬 렌더링 기능으로, UI 응답성을 저하시키지 않으면서 `refresh()`와 `reset()`을 함께 호출할 수 있도록 도와준다.
