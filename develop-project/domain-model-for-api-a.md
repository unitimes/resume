# A사 상품 제휴 API 서비스 domain model 개발
## 목적(WHY)
### 비지니스적 목적
A사와 상품 연동하는 신규 서비스 프로젝트의 domain model 개발
### 기술적 목적
- ORM(JPA & Hibernate) 연관관계 성능 최적화
- Domain model의 hierarchy, category 구조 고도화
- 역할 기반 객체 분리 고도화
## 구현 특이사항(HOW)
### 복수건 쿼리 성능 최적화
연관 관계에 대한 lazy loading 불가 시 join 비용 항상 발생.
N:1 관계에서 FK를 가지고 있지 않을 경우 outer join 가능성 높음.
**Hibernate bytecode enhancement 통해 모든 연관관계 lazy loading 구현**
### 객체 분리 고도화 및 타입 관계 고도화
- @Embeddable 활용하여 역할 기반 객체 분리
- extends 활용하여 domain model간 hierarchy 구현
- interface implement 활용하여 hierarchy관계 아닌 domain model들 type categorizing
## 얻은 것(RESULT)
### Business context separation에 대한 이해
단순 상품 연동 business context에서 상품 model은 DTO 수준의 역할만을 수행. 상품 type별 상이한 business logic 불필요한 상황에서 상품 type별 객체 정의는 과도한 리소스 투입일 수 있음

**_적절한 규모의 business context 분리 _**

과도한 세분화는 domain model 관리 비용을 증가시킴

> e.g. Db schema 수정 시 해당 table을 사용하는 모든 context의 domain model 수정 필요

적절한 business context 분리가 없을 경우 너무 많은 책임을 갖는 domain model 객체가 생길 수 있고 이는 관리 비용 증가로 이어짐

> e.g.1 의미가 불분명한 인터페이스 출현 가능성이 높음. context 별로 인터페이스가 가지는 의미가 다야할 수 있기 때문.
> e.g.2 여러 곳에서 의존성을 갖는(참조하는) domain model 객체의 인터페이스는 수정이 어려움(유지 보수 비용의 증가)

### DB 리팩토링의 필요성
DB 스키마와 domain model 간 디커플링은 ORM의 이점을 반감

- 적절한 type 분리 불가
- 연관관계 설정에 과도한 비용 발생
- 적절한 역할 부여와 이의 구현이 어려움

### Hibernate 버전 별 구현의 상이함
Bytecode enhancement 사용 시 실제 작동 방식이 버전별 상이 할 수 있음.
적절한 hibernate configuration을 통해 실제 작동 방식 확인 필요

> hibernate.generate_statistics 적용하면  connection, query statement, 수행 시간 등의 정보 log 출력
