---
title: "데이터 이력관리"
categories:
  - Spring Boot
tags:
  - Spring Boot
  - Jpa
  - Gradle
---

## 개요
- 프로젝트를 수행하다 보면 데이터 이력관리가 필요합니다.
Envers는 데이터의 추가, 수정, 삭제에 대한 모든 이력을 Entity 기준으로 자동으로 관리해 주기 때문에 이러한 고민을 덜어주는 아주 훌륭한 라이브러리입니다.

## Envers

- 데이터 변경 이력을 로깅하기 위한 라이브러리입니다.
- JPA 스펙에 정의된 모든 매핑을 감사합니다.
- 엔티티의 변경 이력을 자동으로 관리합니다.

## 설정
1. Build.gradle에 dependency를 추가합니다.
```kotlin
implementation("org.hibernate:hibernate-envers")
```
2. Entity에 @Audited 어노테이션 추가합니다.
```kotlin
@Entity
@Audited
@Table(name = "demo_table")
data class DemoEntity()
```
- demo_table_aud, revinfo 테이블이 생성됩니다.
- revinfo는 central revision table입니다.
- @Audited(withModifiedFlag = true)로 설정하면 aud 테이블마다 수정상태 컴럼이 추가됩니다.
- ex) name -> name_mod
3. Propety Config
```yaml
spring:
  jpa:
    properties:
      org:
        hibernate:
          envers:
            audit_table_suffix: _aud
            revision_field_name: rev 
            revision_type_field_name: revtype
            store_data_at_delete: true
```
- audit table의 prefix, suffix를 수정할 수 있습니다.
- revision_field, type의 name 수정 가능합니다.
- delete시 aud테이블에서 타겟 테이블의 pk만 쌓을뿐 다른 필드의 값은 기본적으로 null입니다.
- null이 아니라 delete 직전의 모든 필드의 값을 쌇고 싶다면 true로 설정합니다.

:::note 주의사항
Property 수정 시 테이블의 이름, 컬럼을 수정해야합니다. 기본값을 사용하는 것을 권장합니다.
:::

4. CustomRevisionEntity
```kotlin
@Entity
@RevisionEntity
class CustomRevisionEntity : Serializable {
    @Id
    @GeneratedValue
    @RevisionNumber
    private val rev: Long = 0

    @RevisionTimestamp
    private val timestamp: Long = 0
}
```
- revinfo의 pk인 rev컬럼은 기본적으로 int로 되어있습니다.
- 데이터가 20억개 이상 넘어가면 오류가 발생하므로 rev를 long으로 변경합니다.

:::note 참고 사이트
Hibernate envers 공식 사이트 : [https://hibernate.org/orm/envers/](https://hibernate.org/orm/envers/)  
:::
