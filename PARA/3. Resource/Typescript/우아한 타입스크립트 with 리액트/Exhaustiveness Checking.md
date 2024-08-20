모든 케이스에 대해 철저하게 타입을 검사하는 것을 말하며 타입 좁히기에 사용되는 패러다임 중 하나이다.
해당 케이스에서 값이 하나라도 분기 처리를 하지 않게되면 컴파일 타임에러가 발생하게 된다.

```ts
type ProductPrice = '5000' | '10000' | '20000' | '30000';

  

const getProductName = (productPrice: ProductPrice): string => {

	if (productPrice === '10000') return '상품권 1만 원';
	if (productPrice === '20000') return '상품권 2만 원';
	if (productPrice === '30000') return '상품권 3만 원';
	if (productPrice === '5000') return '상품권 5천 원';
	
	else {
		exhaustiveCheck(productPrice);
		return '배민 상품권';
	}
};

  

const exhaustiveCheck = (param: never) => {
	throw new Error('type error!');
};
```
