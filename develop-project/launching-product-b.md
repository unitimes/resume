# B상품 신규 출시

##목적(WHY)
### 비지니스적 목적
신 상품 출시로 매출 증대
### 기술적 목적
- Persistence framework 활용하여 연관관계, 상속 등 구현
- 상품 상속관계 및 다형성 활용하여 구매 로직 추상화
## 구현 특이사항(HOW)
### Mybatis mapper xml
- discriminator, extends 활용하여 상속관계 구현
- association, fetchType 활용하여 연관관계, lazy loading 구현
### 구매 로직 추상화로 중복 로직 제거
상품 상속관계 활용하여 전략 패턴 적용
### 인터페이스에 사용하는 primitive type을 domain model로 전환
> e.g. order id로 int 사용 -> int field 가진 OrderId 객체로 전환

## 얻은 것
### Persistence framework(mybatis) 활용한 domain model 구현
 mapper xml를 활용(연관관계, 다형성 등 활용)하여 비지니스 로직을 수행할 수 있는 domain model 구현
 ### field vs. parameter
 객체가 역할 수행에 필요한 모든 데이터를 field로 가지면 과도한 의존관계가 생겨 관리 비용이 증가 함. 객체 설계 상 객체의 속성에 해당하는 데이터만 field로 설정하고 역할 수행에 필요한 데이터는 parameter로 전달해야 함.
