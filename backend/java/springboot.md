## Spring Boot
Spring Framework를 기반으로 의존성, 서버 설정 등을 자동화 하고 내장 웹서버(tomcat)를 제공하여 <br/>
Java application을 빠르고 쉽게 만들도록 하는 프레임워크

## JPA (Java Persistence API)
Java에서 객체(entity)와 database를 연결하는 표준 API <br/>
@Entity @Id와 같은 어노테이션을 사용하여 객체와 테이블을 매핑하는 규칙 정리 <br/>
JPA는 설계도 역할 -> 반드시 구현체 필요  ex) `Hibernate`,`EclipseLink`,`OpenJPA`,`DataNucleus`
- Spring Data JPA <br/>
  - Spring Framework와 결합하여 JPA를 더욱 간편하게 활용하도록 도와주는 라이브러리 
  - JpaRepository를 상속받은 인터페이스 만들어서 처리

    ```xml
    public interface UserRepository extends JpaRepository <User, Long> { } 
    ```
  - JPA 사용 시 Repository에 필요한 메서드를 하나하나 정의해야 하지만 기본적인 CRUD 작업 자동 제공 (구현 코드 없이 메서드 자동 생성) <br/>
    ex) `findById()` , `save()` ,`deleteById()`
  - 쿼리메서드 제공 (메서드 이름으로 쿼리 생성)  <br/>
    ex) `findByEmail()` : email을 기준으로 조회하는 쿼리 생성
  - Pageable, Sort 를 사용하여 손쉽게 페이징처리 , 정렬 구현 가능
    
    ```xml
    public List<FoodEntity> getFoodList (String foodname, int page, int size) {
       Pageable pageable = PageRequest.of(page, size);  // 페이지 번호(page)와 페이지 크기(size) 설정
       return foodListRepository.findByFoodName(foodname, pageable);  // 지정된 이름으로 음식 리스트를 페이징하여 조회
    }
    ```
- Hibernate
  - 가장 일반적으로 사용되는 JPA 구현체, Spring Data JPA의 기본 구현체
  - JPA에서 정의한 표준을 구현하여 실제 데이터베이스와 상호작용을 처리해주는 라이브러리 <br/>
    (JPA annotation에 맞춰 그 규칙을 실제 동작하도록 만드는 코드라고 볼 수 있음)


