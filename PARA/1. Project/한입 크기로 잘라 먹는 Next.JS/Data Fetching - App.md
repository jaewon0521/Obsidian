

#### Data Cache
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

#### cache
- `force-cahce` : 캐싱 한다는 옵션 입니다.
- `no-store` :  캐싱하지 않는다는 옵션 입니다.

#### next
- `revalidate` : 특정 시간을 주기로 캐시를 업데이트 합니다. Page Router의 ISR 방식과 유사합니다.
- `tags` : On-Demand Revalidate 요청이 들어왔을 때 데이터를 최신화 합니다.

