
1. 라우팅 : pages/search.tsx or pages/search/index.tsx
2. 동적 라우팅 : pages/book/[id].tsx 
3. Catch ALL Segment : pages/book/[...id].tsx -> /book/123/456/789 
	1. query는 배열 형태로 전달이 됩니다.
4. Optinal Catch All Segment : pages/book/[[...id ] ] -> /book
	1. 404 Page가 발생하지 않고 범용적인 경로로 사용할 수 있습니다.
5. 