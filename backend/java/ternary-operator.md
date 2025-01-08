## 삼항연산자 -> 조건연산자
코드의 가독성을 높이고 간단한 조건문을 더 간결하게 작성 <br/>
```
  조건식 ? 참일 때 실행할 코드 : 거짓일 때 실행할 코드;
```
ex) home화면에서 보여 줄 체중 가져오는 코드 
<br/>
-> 우선 순위에 따라 weight 값을 설정 <br/>
<br/>
1순위 weightEntity.getWeight() <br/>
2순위 latestWeightEntity.getWeight() <br/>
3순위 userProfileWeight <br/>

```xml
  // 조회 날짜의 weight
  WeightEntity weightEntity = weightService.getWeightRecord(email, start, end); 
  
  // 조회 날짜 이전 가장 최신의 weight
  WeightEntity latestWeightEntity = weightService.getLatestWeightBeforeDate(email,start);
  
  // 체중 기록이 아예 없다면 가입 시에 userProfile에 등록한 weight
  float userProfileWeight = userProfile.get().getWeight();

   float weight = (weightEntity != null) ? weightEntity.getWeight() :
                  (latestWeightEntity != null) ? latestWeightEntity.getWeight() : userProfileWeight ;
```

#### * 삼항 연산자는 값을 반환하거나 변수를 할당할 때 사용 -> void 메서드에는 부적합 
ex) parameter bodyFat이 null 일 때 기존 entity의 bodyFat값 설정하고, <br/>
  parameter 존재시 받아온 bodyFat 값 설정하는데 set 메서드는 사용 하지 않음

  <br/> -> set 메서드에 사용 불가능 <br/>
  ```xml
  (bodyFat == null) ? weightEntity.setBodyFat(weightEntity.getBodyFat) :
                      weightEntity.setBodyFat(bodyFat) ; 

  ```
 <br/> -> 삼항연산자로 값을 변수에 할당 후 setting <br/>
   ```xml
 float bodyFat = (bodyFat == null) ? weightEntity.getBodyFat : bodyFat ;
 weightEntity.setBodyFat(bodyFat); 

  ```
