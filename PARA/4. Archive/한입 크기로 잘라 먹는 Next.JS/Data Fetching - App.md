#### Data Cache
- 백엔드 서버로부터 불러온 데이터를 반 영구적으로 보관하기 위해 사용된다.
- 서버 가동죽에는 영구적으로 보관된다.
``` typescript

// cache
const apiFetch = async() => {
	const res = await fetch('~/api', { cache : 'force-cache' });
}

// force-cache
// no-store

// next
const apiFetch = async() => {
	const res = await fetch('~/api', { next : { revalidate : 3 }});
}

// revalidate
// tags
```

##### cache
- `force-cahce` : 캐싱 한다는 옵션 입니다.
- `no-store` :  캐싱하지 않는다는 옵션 입니다.

##### next
- `revalidate` : 특정 시간을 주기로 캐시를 업데이트 합니다. Page Router의 ISR 방식과 유사합니다.
- `tags` : On-Demand Revalidate 요청이 들어왔을 때 데이터를 최신화 합니다.

#### Request Memoization
- 하나의 페이지를 렌더링하는 동안에만 존재하는 캐시입니다. 렌더링이 과정이 종료가 되면 Request Memoization의 캐시는 모두 소멸 된다.
- 하나의 페이지에서 동일한 API 요청이 발생하게 되면 사전 렌더링 시점에 Request Memoization을 발생 시켜 데이터를 캐시 한다.
- First Request : Request Memoization (Miss) -> 백엔드 서버 -> Request Memoization (Set)
- 이후에는 캐시가 저장된 Request Memoization에서 캐시된 데이터를 반환 한다.
- 중복된 API 요청을 방지하는 목적 이다.

#### App Router vs Page Router Data Fetching
- Page Router 에서는 서버 측에서만 실행되는 함수를 통해서 데이터를 Props로 넘겨주어야 하는 불편함이 있다.
- App Router 에서는 컴포넌트가 각각 자신이 필요한 데이터를 직접 불러오는 방식으로 진행한다.
