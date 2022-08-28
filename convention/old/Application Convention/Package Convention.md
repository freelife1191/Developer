# 도메인 패키지

```
└── domain
    ├── dto
    │   ├── model
    ├── entity
    │   ├── model
    ├── enums
    ├── vo
    └── process
    └── pub
    └── sub
```

도메인 패키지들은 domain이란 그룹 패키지로 묶는다

- dto: 데이터 요청/응답 객체를 담는 패키지
  - model: DTO 객체 구성시 필요한 별도의 내부 그룹 객체를 담는 패키지
- entity: JPA/Mongo Entity 객체를 담는 패키지
  - model: JPA/Mongo 객체 구성시 필요한 별도의 내부 그룹 객체를 담는 패키지
- vo: VO객체를 담는 패키지
- enums: enum 객체를 담는 패키지
- process : 그외 커스텀하게 사용하기위한 도메인 객체들을 담는 패키지
- pub: 데이터 Producing 객체를 담는 패키지
- sub: 데이터 consuming 객체를 담는 패키지

 
```
└── domain
    ├── dto
    │   ├── model
    │   │   └── SampleAddressDTO.kt
    │   ├── ReqSampleSaveDTO.kt
    │   ├── ReqSampleSearchDTO.kt
    │   ├── ReqSampleUpdateDTO.kt
    │   ├── ResSampleSaveDTO.kt
    │   ├── ResSampleSearchDTO.kt
    │   └── ResSampleUpdateDTO.kt
    ├── entity
    │   ├── model
    │   │   └── SampleAddress.kt
    │   └── Sample.kt
    ├── enums
    │   └── SampleEnum.kt
    ├── vo
    │   └── SampleVO.kt
    └── process    
       └── Sample.kt
    └── pub    
       └── SamplePub.kt
    └── sub    
       └── SampleSub.kt
```

- dto
  - 데이터 요청/응답 객체를 정의
  - controller 를 기준으로 별도로 생성해야함
  - 요청시 Prefix - Req, 응답시 Prefix - Res
  - suffix - DTO  (반드시 대문자)
- entity
  - Table 명 기준으로 Entity 객체 생성
  - JPA Entity와 Properties 를 정의
- model
  - Entity와 DTO 내부 구성 객체 정의
  - 예를 들어 Address 객체 와 같이 우편 번호, 주소 등의 Properties 들을 묶어놓은 객체
- vo
  - 불변형 객체 ( Setter 가 없고 Constructor 로만 값을 셋팅 )
  - Entity 객체의 Properties 와 동일하지만 일부 Properties가 추가될 수 있음
  - 데이터 처리를 위한 메서드가 추가될 수 있음
  - Equals 와 HashCode 메서드가 추가될 수 있음
- process
  - Entity와 DTO에 종속되지 않고, 서비스 로직에 필요한 객체 정의
  - 도메인 객체는 process 패키지 하위에 정의
  - process 하위에 서비스 별 하위 패키지 생성하여 구분 가능 ( optional )
- pub/sub
  - Producing, Consuming 데이터 객체 정의 
  - Kafka에서 사용