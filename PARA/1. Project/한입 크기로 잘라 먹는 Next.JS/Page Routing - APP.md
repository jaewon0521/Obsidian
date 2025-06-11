
#### Routing
1. /app 하위 폴더에 경로 폴더를 생성후 page.tsx를 설정 합니다.  /app/search/page.tsx -> /search

나머지 라우팅은 Page 라우터와 동일합니다.

#### Layout
1. 폴더 내부에 layout.tsx파일을 생성합니다. 해당 폴더로 시작하는 모든 경로는 UI를 공유 하게 됩니다.

#### Route Group
1. 폴더는 라우팅 구조를 정리할 때 쓰는 폴더지만, URL 경로에는 포함되지 않습니다.
	1. /(admin)/dashboard -> /admin/dashboard (x) /dashboard (o)
- 코드를 **논리적으로 그룹화**하기 위해서 사용
- 예를 들어, 다음과 같이 관리자용과 사용자용 페이지를 나눌 수 있다.
    
`app/ 
├── (admin)/ │   └── dashboard/ ├── (user)/ │   └── dashboard/`

이 경우, URL은 둘 다 다음처럼 됩니다:

- `/dashboard` → 각각 (admin)과 (user) 중에서 layout 등으로 구분 가능
    

> 즉, 폴더 구조는 나누지만 **URL 경로에는 영향을 안 주므로 클린한 URL을 유지**할 수 있어요.