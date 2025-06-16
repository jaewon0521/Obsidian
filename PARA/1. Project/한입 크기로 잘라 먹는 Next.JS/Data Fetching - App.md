

#### Data Cache
``` typescript

const apiFetch = async() => {
	const res = await fetch('~/api', { cache : 'force-cache' });
}

// force-cache
// no-store


```
- `force-cahce` :  fetch 함수의 두번째 인자로 cache : 'force-cache'를 입력하게 되면 최초로 한번 호출하고 나서 다시는 재 호출 하지 않습니다.
- `no-store` : 캐싱하지 않는다는 옵션 입니다.
- 