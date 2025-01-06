## JPA 쿼리 파라미터 바인딩 방식


###  1. JPQL (Java Persistence Query Language)
실제 테이블 대신 객체 관계 매핑 ORM(Object Relational Mapping)을 통해 테이블과 연관된 Entity 객체를 사용하여 DB에 Query를 실행 <br/>
  - 인덱스 기반 파라미터 바인딩 (Position-based Binding) <br/>
     쿼리에서 ?1, ?2와 같은 형식으로 파라미터를 지정하고 순서대로 바인딩 <br/>
     
	 ```xml
      @Query("UPDATE UserProfile u SET u.targetWeight =?2 WHERE u.email =?1")
      void updateTargetWeight(String email, float targetWeight);
    ```
  - 명명된 파라미터 바인딩 (Named Parameter Binding) <br/> 
     파라미터이름 형식으로 파라미터를 정의하고 @Param 어노테이션을 사용하여 파라미터의 이름 명시적 지정 <br/>
     @Param 생략 시 @Query에서 자동으로 파라미터 이름 추론 <br/>

     ```xml
      @Query("SELECT *  u FROM users u WHERE u.email = :email")
      UserEntity findUser(@Param("email") String email);
    ```

### 2. Native SQL 
네이티브 SQL을 실행하며, ?1, ?2로 메소드 파라미터를 바인딩 (Entity가 아닌 DB테이블을 직접 참조 -> Entity 객체 필드명이 아닌 테이블의 컬럼명을 <br/>
기준으로 쿼리 작성, 실제 쿼리문법 사용, 인덱스, 파라미터 모두 사용 가능)

 ```xml
  @Query(value = "SELECT * FROM users WHERE email = ?1", nativeQuery = true)
  UserEntity findUser(String email);
 ```                                      
  ### 3. Query method 
  이름 규칙을 이용한 바인딩 <br/>
  
  `List<FunctionalFoodEntity> findByProductNameContaining (String productName)` =

  `SELECT * FROM FunctionalFoodEntity WHERE product_name LIKE '%productName%';`
    
