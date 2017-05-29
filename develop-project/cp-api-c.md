# C사 상품 제휴 API 서비스 개발
## 목적(WHY)
### 비지니스적 목적
상품 물량 증대
### 기술적 목적
- Gradle multi project build 생성
- Spring boot 적용
- DDD 적용하여 프로젝트 구조화
- ORM 적용(JPA + Hibernate)
- message source 사용
	- data validation 결과 message 전송 시 사용
- Spring security 적용
	- 외부 통신 authentication, authorization
- Validation data
## 구현 특이사항(HOW)
### Spring Java config
### properties 파일 관리
Gradle 멀티 프로젝트 빌드 시, 빌드 주체 프로젝트(maven의 module)의 application.properties가 다른 프로젝트의 application.properties를 덮어 씌워 설정이 적용되지 않음.

**_spring.profiles.include property 활용하여 관리_**

각 프로젝트 별 고유 프로파일을 부여.
실제 설정 properties는 application-{프로젝트 고유 프로파일}.properties에 작성.
build 주체 프로젝트의 application.properties 파일에 spring.profiles.include 설정하고 사용(연관관계)할 프로젝트의 고유 프로파일 나열.
### javax.validation 확장
javax.constraints 외 NotNullNotIf, NullNotIf constraints 추가.
interface간 extends 사용하여 validation group에 hierarchy 적용하여 활용.
### HMAC 활용한 Authentication, Authorization, Data integrity check
로그인 적합하지 않은 비지니스 환경.

**_Authentication, Data integrity check_**

Request의 Authorization header에 사용자 정보와 전송 데이터 일부를 사용자 고유 secret key로 암호화하여 첨부. 서버에서 동일한 암호화 과정을 거쳐 request의 Authorization header의 값과 비교.

**_Authorization_**

Spring security의 Authorities 활용
## 얻은 것(RESULT)
### Spring security 프레임워크 인터페이스 구현
스프링에서 제공해 주는 구현체 중 비지니스에 적합한 구현체 부재로 직접 구현.
직접 구현하는 경우, 보다 많은 프레임워크 이해 비용 필요. 그러나 프레임워크를 적용함으로써 다른 프레임워크와의 통합성 높일 수 있음. 또한 세션관리, 필터 오더링, handler method authorization, 기타 보안 설정 관리 등이 편리 해짐.
### Business context 중심적인 domain modeling 필요
JPA 적용에 있어 lazy loading 구현 위해 많은 자원 투입.

- Ant task 통해 hibernate의 instrument task 수행하여 구현

외부로부터 상품을 등록하는 API 서비스로 domain model을 reading 하는 작업 미미하여 lazy loading 구현의 이점이 적음. Lazy loading 구현은 과도한 비용 투입일 수 있음.
