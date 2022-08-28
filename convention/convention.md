## 컨벤션

- 클래스나 변수명은 fullname 을 지향
- private 내부변수나 내부 메소드의 경우 최상단에 배치
- 메서드명의 마지막단어는 복수형일 경우 복수형으로 관리 (createStreamArchiveKeyframes)
- 단어는 동사의 흐름대로 (flush)
- 공통 객체나 모듈은 Common 영역에 배치
- 단수형 조회는 `find` 복수형 조회는 `findAll`
- Loop문을 돌면서 처리하는 로직에서는 DB커넥션 처리하는 로직 처리시 Loop문 밖에서 Loop문에서 사용되는 데이터를 추출후 Map형태로 재가공하여 사용

Stream Map으로 추출하는 예시

```java
List<CampaignPopupEntity> campaignPopupEntities = campaignPopupDao.findAllByCampaignIdAndPopupType(campaignId, PopupType.QUIZ);
Map<Long, PopupResourceEntity> popupResourceEntityMap =
    popupResourceDao.findAll(campaignPopupEntities.stream()
            .filter(campaignPopupEntity -> campaignPopupEntity.getPopupResourceSnapshot() == null)
            .map(CampaignPopupEntity::getPopupResourceId).collect(toSet()))
        .stream()
        .collect(toMap(PopupResourceEntity::getId, Function.identity()));
// 모든 상태의 퀴즈 리스트를 생성
return campaignPopupEntities.stream()
    .map(campaignPopupEntity -> adminCampaignPopupObjectBuilder.buildCampaignPopup(
        campaignPopupEntity,
        campaignPopupEntity.getPopupResourceSnapshot() == null ? popupResourceEntityMap.get(campaignPopupEntity.getPopupResourceId()) : campaignPopupEntity.getPopupResourceSnapshot(),
        false))
    .map(CampaignPopup::getPopupResource)
    .map(PopupResource::getContents)
    .map(popup -> ((Quiz) popup).setRightPopup(null).setWrongPopup(null).setWrongDisplayType(null))
    .collect(toList());
```

## DB 컨벤션

컬럼명은 '_' 없이 소문자 영문으로만
컬럼명이 두개 이상일 경우 '_' 를 붙이고 다음 컬럼명

### Unique Key 생성규칙

uk_컬럼명1_컬럼명2

예) uk_campaignid_onair

### Index 생성규칙

k_컬럼명1_컬럼명2

예)  k_livestreamstatus