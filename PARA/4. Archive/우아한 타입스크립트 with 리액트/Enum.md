#### Enum vs Union
- Enum은 트리 쉐이킹이 안되서 번들 사이즈에 영향을 줄 수 있다.
- Enum은 값이기 때문에 이터러블 하다.
- Enum은 가독성이 Union Type에 비해 더 좋은 점이 있다.

#### Three Shaking 관점
- Union Types > const enum > const object > enum