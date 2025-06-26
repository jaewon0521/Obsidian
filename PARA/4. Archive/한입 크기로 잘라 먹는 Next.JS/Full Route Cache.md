
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

#### 라우트 세그먼트 옵션
라우트 세그먼트 옵션은 특정 페이지의 동작을 강제적으로 설정할 수 있는 옵션이다. 이를 통해 캐싱 설정, Revalidate 타임, 서버 지역 등을 지정할 수 있다.

##### dynamicParams 옵션
- `dynamicParams: false`: `generateStaticParams`에서 반환된 경로 외의 URL 요청은 모두 404 페이지로 리다이렉트.
- `dynamicParams: true` (기본값): `generateStaticParams` 외의 경로도 실시간으로 생성.
``` typescript
 export const dynamicParmas = false;
```
##### dynamic 옵션 (페이지 유형 강제 설정)
- `auto`: 기본값. 페이지를 구성하는 컴포넌트의 동작에 따라 Static 또는 Dynamic 페이지로 자동 설정.
- `force-dynamic`: 페이지를 강제로 Dynamic 페이지로 설정.
- `force-static`: 페이지를 강제로 Static 페이지로 설정.
- `error`: 페이지를 강제로 Static 페이지로 설정하되, 동적 함수나 캐싱되지 않은 데이터 페칭이 있다면 빌드 오류 발생

``` typescript
 export const dynamic = 'force-dynamic';
```

> [!error] 
> `force-static`을 사용할 때, 동적 함수를 사용하는 데이터를 불러오는 경우에는 검색 기능이 동작하지 않을 수 있다.
> searchParams, params
