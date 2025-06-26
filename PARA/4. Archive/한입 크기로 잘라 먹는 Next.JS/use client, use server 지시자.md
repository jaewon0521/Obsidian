#### use client
컴포넌트가 클라이언트에서 실행됨을 선언하는 지시자이다.
#### use server
 컴포넌트가 서버에서 실행됨을 선언하는 지시자이다.
#### use client는 CSR에서만 실행이 되는가?
답변은 아니다. SSR에서도 실행이 되며, SSR 시점에 HTML 프리 렌더링 된다.
컴포넌트 파일에 use client를 선언하게 된다면, return 문의 JSX 코드는 SSR 시점에 HTML로 변환하며
파일 내부의 hook, 이벤트 핸들러는 react 런타임 환경 즉 클라이언트 환경에서 실행이 된다.

그렇기 때문에 HTML + JS Bundle을 전송한다.
응답받은 HTML을 기반으로 JS Bundle 파일을 DOM에 hydration을 진행한다.
#### use client 컴포넌트에서 use server 함수 호출은 가능한가 ?
답변은 가능 하다. 단, 직접 호출은 안 되며, form action으로 간접 호출 하여야 한다.
클라리언트 번들에는 서버 코드는 포함하지 않기 때문에, 직접 호출은 안 되고, form action과 같은 Next가 제공하는 특수 인터페이스를 통해서만 호출이 가능하다.

``` tsx
// /action/submit-form.ts
'use server';
export async function submitForm(data: FormData) {
	const name = data.get('name');
}

// /page.tsx
'use client';
export default function PageForm() {
	return (
	<form action={submitForm}>
		...
		<button type='submit'>전송</button>
	</form>)
}

```
##### 동작 방식
유저가 form을 제출하면 Next.js는 해당 RFC 주소로 요청을 보내고, 서버에서 submitForm을 실행 한다.

