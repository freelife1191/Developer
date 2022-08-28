## Spec
- MongoTemplate : Spring-data-mongodb에서 제공해주는 mongoTemple 사용.
  - querydsl-mongo : 집계 기능을 사용하지 못하여 제외

## Naming Convention
| **Target**      | **Naming Rule** | **e.g.**                 |
| --------------- | --------------- | ------------------------ |
| Collection      |                 |                          |
| Field           | Lower Camel     | companyName, storeName.. |
| Reference Field | Snake           | `<document>_id`          |

## Reference
- https://docs.mongodb.com/manual/reference/database-references/#document-references