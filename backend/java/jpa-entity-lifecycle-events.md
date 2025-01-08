## JPA Entity Lifecycle
entity 객체가 생성되고 DB에 저장되며, 수정되고, 삭제되는 전체 과정

|단계|설명|
|------|---|
|Transient (일시적, 비영속) |entity가 생성되었지만 아직 DB에 저장되지 않은 상태 <br/> 영속성 컨텍스트에 의해 관리되지 않는 상태 <br/> new 상태의 객체에 해당|
|Persistent (영속적) |entity가 영속성 컨텍스트에 의해 관리되는 상태 <br/> DB와 동기화, save 호출 상태에 해당 |
|Detached (분리됨, 준영속) |entity가 영속성 컨텍스트에서 분리된 상태  <br/> 트랜잭션 종료시 |
|Removed (삭제됨) |entity가 영속성 컨텍스트에서 제거되었고 DB에서 삭제예정 <br/> 트랜잭션 종료시 DB에서 실제로 삭제됨 |
```xml
* 영속성 컨텍스트(Persistence Context)란?
JPA에서 entity를 관리하기 위해 entityManager가 제공하는 일종의 가상데이터 저장소

역할
1차캐시 - 처음 조회된 entity를 메모리에 저장 후 동일한 트랜잭션 내에서 다시 조회시
          DB에 접근하지 않고 메모리에 저장된 entity를 반환 
변경감지 (Dirty Checking) - 트랜잭션 커밋시 변경된 entity를 자동으로 DB에 반영
트랜잭션 관리 - 트랜잭션과 함께 동작 -> 트랜잭션 시작시 생성되고, 커밋되거나 롤백시 종료됨
```

## JPA Lifecycle callback annotation
entity의 생명 주기 동안 특정 이벤트가 발생할 때 자동으로 호출되는 메서드를 정의하는 어노테이션 <br/>

- @PrePersist - entity가 DB에 삽입되기 전 호출
- @PreUpdate - entity가 DB에서 업데이트 되기 전 호출
- @PostPersist - entity가 DB에 삽입된 후 호출
- @PostUpdate - entity가 DB에서 업데이트 된 후 호출 
- @PreRemove - entity가 DB에서 삭제되기 전 호출 
- @PostRemove - entity가 DB에서 삭제된 후 호출
- @PostLoad - entity가 Load된 후 호출 (find, 조회된 후 호출)

ex) entity 저장 전 생성시간, 업데이트 시간 컬럼을 현재로 설정 <br/>
entity 상태 변경과 관련된 내부로직을 처리하는 것이므로 외부에서 호출 할 필요가 없음 <br/>
-> private, protected로 접근제어자 설정, public 설정시 의도치 않은 업데이트 발생 가능 <br/>
entity를 상속하여 사용하는 경우에는 protected 사용

```xml
    @PrePersist
    Protected void onCreate() {
       LocalDateTime now = LocalDateTime.now();
       this.createdAt = now;
       this.updatedAt = now;
    }
```





