## API 디자인 규칙

1. 외부연동
   1. `/api/v1/category/subcategory/name-sub`
      1. prifix - `/api/v1/`
      2. 기획서나 table 기준으로 메인 `category` 지정
      3. 특정 업무 그룹에 따라 `subcategory` 추가 예를 들어 account 패키지내에 supplier 패키지의 Controller라면 `/api/v1/account/supplier` 로 표현할 수 있음
      4. `name` 은 최대한 심플하게 생성할 수 있도록 하고 불가피 할시 `-` 사용하여 `subname` 을 추가
      5. API 작성시 URL은 단수형으로만 사용
2. 내부연동
   1. /api/v1/category/**internal**/subcategory/name-sub
   2. /api/v1/category/{category_id}/**internal**/subcategory/name-sub

## 요청/응답 객체 생성 규칙

1. Front 에 제공되는 API의 Properties 는 모두 SnakeCase
   1. SnakeCase 지만 내부적으로는 CamelCase 이고 Jackson 라이브러리나 Converter를 이용해 변환처리 되도록 해야함

# Swagger Api Docs Convention

## `@Tag` 작성 규칙

내부 연동과 외부 연동으로 나눠 작성한다.

### 외부연동

```
@Tag(name = “[Schema] Group”, description = “GroupController”)
```

Swagger Api Docs Tag 명 규칙은 앞에 `[Schema]` 명으로 구분할 수 있도록 하고 그 뒤에 그룹명을 작성한다

ex) `@Tag(name = “[상품] 상품”, description = “ProductController”)`

### 내부연동

```
@Tag(name = “[Schema-내부연동] Group”, description = “GroupInternalController”)
```

Swagger Api Docs Tag 명 규칙은 앞에 `[Schema-내부연동]` 명으로 구분할 수 있도록 하고 그 뒤에 그룹명을 작성한다 

ex) `@Tag(name = “[상품-내부연동] 상품”, description = “ProductInternalController”)`