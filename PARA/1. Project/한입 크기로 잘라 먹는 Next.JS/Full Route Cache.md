
#### 1. 개요
Next 서버 측에서 빌드 타임에 특정 페이지의 렌더링 결과를 캐싱하는 기능이다.

/a 라는 주소를 가진 페이지의 fetch data를 리퀘스트 메모이제이션, 데이터 캐시 기능을 거쳐 생성한 결과를 풀라우트 캐시에 저장한다.

브라우저 접속 요청이 들어 왔을 때 풀 라우트 캐시에서 캐시 된 결과를 꺼내와 사용자에게 빠른 페이지를 보여준다.


Next에서는 정적 페이지와 동적 페이지로 어떤 기능을 사용하느냐에 따라 자동으로 나뉜다.
자동으로 정적 페이지에서는 풀 라우트 캐시가 적용 된다.


#### 기준
##### Dynamic Page 기준
페이지가 접속 요청을 받을 때 마다 매번 변화가 생기거나, 데이터가 달라질 경우

1. 캐시되지 않는 Data Fetching 을 사용할 경우
``` typescript
await fetch('/url', { cache : 'no-store' })
```
2. 동적 함수 (쿠키, 헤더, 쿼리스트링)을 사용하는 컴포넌트가 있을 경우
``` tsx

// cookie, headers
import { cookies } from 'next/headers';

async function Component() {
	const cookieStore = cookies();
	const theme = cookieStore.get('theme');
	
	return <div>...</div>
}

// queryString
async function Page ({ searchParams } : {searchParams: { q: string }}) {
	const q = searchParams.q;

	return <div>...</div>
}
```

##### Static Page 기준
Dynamic Page가 아니면 모두 Static Page가 된다. (Default)


#### 비교
| 동적 함수 | 데이터 캐시 |    페이지 분류    |
| :---: | :----: | :----------: |
|   O   |   X    | Dynamic Page |
|   O   |   O    | Dynamic Page |
|   X   |   X    | Dynamic Page |
|   X   |   O    | Static Page  |

