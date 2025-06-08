
#### Routing
1. 라우팅 : pages/search.tsx or pages/search/index.tsx
2. 동적 라우팅 : pages/book/[id].tsx 
3. Catch ALL Segment : pages/book/[...id].tsx -> /book/123/456/789 
	1. query는 배열 형태로 전달이 됩니다.
4. Optinal Catch All Segment : pages/book/[[...id ] ] -> /book or /book/123 or /book/123/456
	1. 404 Page가 발생하지 않고 범용적인 경로로 사용할 수 있습니다.

#### Pre-Fetching
프리페칭은 프로덕션 환경에서만 활성화 됩니다.
yarn build - yarn start

- 현재 접근한 페이지에서 다른 경로로 갈 수 있는 경로는 모두 사전에 JS 파일을 Load 합니다. (Link Component)
- Link Component에 prefetch Props로 Boolean 값을 넘겨줍니다.
- JS 파일은 Disk에 캐시됩니다.
- useRouter 객체는 router.prefetch로 설정합니다.
``` tsx
useEffect(() => {
	router.prefetch('/경로');
},[]);
```





