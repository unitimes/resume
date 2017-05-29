# 퍼시스턴스 레이어 분리
## 목적(WHY)
### 특정 퍼시스턴스 레이어 기술 노출 지양
퍼시스턴스 프레임워크로 ibatis, mybatis 모두를 혼용하고 있는 환경. 각 기술에 특화된 패턴과 구조를 가진 객체(ibatis-DAO, mybatis-Mapper)를 어플리케이션 전반에 거쳐 직접 의존성을 가짐.
기반 기술 은닉화를 통해 유지 보수 편이성 증대시킬 목적
## 구현 특이사항(HOW)
### Repository interface로 기반 기술 은닉화
Repository interface 참조로 퍼시스턴스 레이어 분리 및 기반 기술 직접 참조 지양
# 뷰 레이어 DTO 분리
## 목적(WHY)
### Command, Result 객체와 Domain model 객체간 의존성 약화
View layer와 data 통신에 사용되는 객체로 domain model 객체를 직접 사용하거나, domain model 객체를 확장(extends) 하는 등 강한 의존관계를 가지는 환경.
이로 인해 view layer가 두터워 지고, 관점 분리가 제대로 이루어 지지 않아 유지 보수에 어려움이 있음.
## 구현 특이사항(HOW)
### Facade 패턴 적용
View layer와 data 통신만을 목적으로 하는 DTO 객체 생성하여 사용.
## 얻은 것(RESULT)
### Thin view layer
Formatting, structuring 로직 감소.
유저 요구사항 변화에 적은 비용으로 대응할 수 있는 환경 조성.
View layer의 template engine에 적용 했던 로직을 controller layer에서 java 로직으로 대체하여 test coverage 확장 및 디버깅 용이성 향상.