## 1. DTO 설정

- 상단 `@Schema` 의 경우 Swagger Schemas DTO 객체 명칭을 한글로 보여주기위함
- `@field:Schema` 기본 생성자 필드에서는 어노테이션을 선언할 때 `@field:` 를 붙여주어야 어노테이션이 적용된다
- 필요하다면 Field `@Schema`에 title 외에 description 과 example 등의 속성 값을 설정할 수도 있다
  - example 의 경우 Swagger 로 테스트할 시 example에 설정한 값을 예시 값으로 보여줄 수 있다
- DTO의 Field는 기본값이 있는 경우를 제외하고는 null 이 가능한 타입으로 정의하고 Validation 어노테이션을 설정하여 유효성체크를 한다
  - non-null 타입으로 정의하고 null 데이터를 넘기면 Kotlin non-null Exception 이 발생하게 된다

```kotlin
@Schema(title = "다중 품목 조합생성 요청")
@JsonNaming(PropertyNamingStrategies.SnakeCaseStrategy::class)
class ReqItemsCreateDTO (
    @field:Schema(title = "all (true: 전체모드 활성화, false: 전체모드 비활성화)", description = "전체모드")
    val all: Boolean = false,
    @field:Schema(title = "start_date", description = "상품등록일 시작일")
    @field:DateTimeFormat(iso = DateTimeFormat.ISO.DATE)
    val startDate: LocalDate? = null,
    @field:Schema(name = "end_date", description = "상품등록일 종료일")
    @field:DateTimeFormat(iso = DateTimeFormat.ISO.DATE)
    val endDate: LocalDate? = null,
    @field:Schema(title = "상품ID 리스트")
    var productIds: List<String> = listOf(),
    @field:Schema(title = "연동 품목코드", description = "기본값: RANDOM 코드", hidden = true)
    var itemCode: String? = null,
    @field:Schema(title = "자체 품목코드")
    var customItemCode: String? = null,
    @field:Schema(title = "정렬번호")
    var sortNo: Int = 0,
    @field:Schema(title = "판매상태: 판매안함(false), 판매함(true)", description = "기본값: 판매함(true)")
    var sellingFlag: Boolean = true,
    @field:Schema(title = "진열상태: 진열안함(false), 진열함(true)", description = "기본값: 진열함(true)")
    var displayFlag: Boolean = true,
    @field:Schema(title = "재고사용 여부: 미사용(false), 사용(true)", description = "기본값: 사용(true)")
    var stockUseFlag: Boolean = true,
    @field:Schema(title = "재고체크유형: codebook (주문기준/결제기준)", description = "기본값: 주문기준(ORDER)", example = "ORDER")
    var stockCheckType: String = "ORDER",
    @field:Schema(title = "공급가")
    var supplyPrice: Int? = null,
    @field:Schema(title = "판매가")
    var sellingPrice: Int? = null,
) {
    override fun toString(): String {
        return "ReqItemsCreateDTO(all=$all, startDate=$startDate, endDate=$endDate, productIds=$productIds, itemCode=$itemCode, customItemCode=$customItemCode, sortNo=$sortNo, sellingFlag=$sellingFlag, displayFlag=$displayFlag, stockUseFlag=$stockUseFlag, stockCheckType='$stockCheckType', supplyPrice=$supplyPrice, sellingPrice=$sellingPrice)"
    }
}
```

## RequestParameter 타입의 DTO 설정

- `@ParameterObject` 어노테이션을 Class 상단에 지정
- Field는 `@field:Parameter` 로 설정

```kotlin
@ParameterObject
class ReqItemSearchDTO (
    @field:Parameter(description = "상품 ID", hidden = true)
    var id: String? = null,
    @field:Parameter(description = "옵션 선택", example = "{\"옵션 선택\":\"바베큐포크 500g\"}")
    var options: String? = null
) {
    fun setId(id: String): ReqItemSearchDTO {
        this.id = id
        return this
    }

} 
```