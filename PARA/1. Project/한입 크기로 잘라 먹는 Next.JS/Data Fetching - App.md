

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
```
- `force-cahce` : 캐싱 한다는 옵션 입니다.
- `no-store` :  캐싱하지 않는다는 옵션 입니다.
- 