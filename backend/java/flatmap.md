## FlatMap
두 개 이상의 컬렉션을 단일 컬렉션으로 평평하게(flatten) 만들고, 각 요소에 대해 함수를 적용하는데 사용 <br/>
*평탄화 - 여러겹으로 쌓인 데이터를 한 겹으로 만드는 것  


```xml
  // [apple,banana],[carrot] 두개의 리스트를 하나로 묶기
  List<List<String>> listOfLists = Arrays.asList(
                                    Arrays.asList("apple", "banana"),
                                      Arrays.asList("carrot"),
                                   );
  // [ [apple,banana], [carrot] ] 을 stream으로 [apple,banana],[carrot] 꺼내고
  //  flatMap으로 리스트 요소 하나씩 꺼내어 단일 리스트로 만듦 [apple, banana, carrot]
  List<String> flatMapList = listOfLists.stream()
                                        .flatMap(List::stream)
                                        .collect(Collectors.toList());

  => 결과 : [apple, banana, carrot]
  => 두 리스트를 하나의 리스트로 묶지 않고 스트림에 넣는 방법 Stream.of으로도 가능
     List<String> combinedList = Stream.of(list1, list2)
```
### ex) 프로젝트에서 실제 사용했던 예
   - 섭취 등록한 건강기능 식품 목록 intake list를 기반으로 record 객체를 만듦
   - intake_time은 breakfast , lunch , dinner 를 선택할 수 있고 , 로 구분하여 string으로 저장됨
   - intake list를 record 객체로 만들 때 map만 사용하게 되면 intake_time의 개별 record는 만들 수 없음 <br/>
     -> intake_time은 [breakfast, lunch], [breakfast,lunch,dinner] 등 여러개가 될 수 있고 그 개수에 따라 record 객체를 만듦 <br/>
     -> 아침, 저녁에 먹기로 저장된 식품의 intake 객체는 1개지만 record 객체는 2개 만들어져야 함  (time에 따른 for문 or flatmap필요)

ex) intake list에 식품 추가 후 getFunctionalFoodRecordList 메서드에서 record 객체 생성 시 

```xml
  newIntakeList.stream()
    .flatMap(intakeEntity -> {
        // 식품 목록의 intake time을 가져와서 ,로 자르고 배열을 만들어 stream을 만듦
        String[] times = intakeEntity.getIntakeTime().split(",");
        return Arrays.stream(times)
            .map(time -> {
                // time 별로 record 객체 생성
                FunctionalFoodRecordEntity recordEntity = new FunctionalFoodRecordEntity();
                recordEntity.setEmail(email);
                recordEntity.setFunctionalFood(intakeEntity.getFunctionalFood());
                recordEntity.setIntakeTime(time);
                recordEntity.setIntakeDt(date);
                recordEntity.setHasIntake(false);
                
                // 현재 요일에 해당하는 경우만 반환
                if (dividedDayOfWeek(dayOfWeek, intakeEntity.getIntakeFrequency())) {
                    return recordEntity;
                }
                return null; // 해당하지 않는 경우 null 반환
            })
            .filter(Objects::nonNull); // null 값 제거
    })
    .forEach(recordEntity -> {
        newRecordEntityList.add(recordEntity);
        if (!recordEntityList.isEmpty()) {
            functionalFoodRecordRepository.save(recordEntity);
        }
    });
```

  ``` 
  intakeEntity1의 intakeTime 배열  intakeEntity2의 intakeTime 배열  intakeEntity3의 intakeTime 배열
  {   [breakfast,lunch,dinner],         [breakfast],                 [breakfast,dinner]...} 

  flatMap 사용시 하나의 평평한 스트림으로 반환
  [breakfast,lunch,dinner,breakfast,breakfast,dinner] 
 ```

-> 어떻게 entitiy의 정보가 각 entity에 setting 되는가?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;intakeEntity 마다 스트림을 반환하여 map이 실행되기 때문에 time과 관련된 값을 recordEntity 객체에 세팅하는 과정이 유지되므로 <br/> 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;필요한 값들이 올바르게 설정됨

-> for문 사용가능 하지만 가독성과 유지보수성, 병렬 처리 (for문은 순차 처리만 지원) 측면에서 flatmap이 효율적

``` 
String[] times = intakeEntity.getIntakeTime().split(",");
      for (String time : times) {
      	FunctionalFoodRecordEntity recordEntity = new FunctionalFoodRecordEntity();
      	recordEntity.setEmail(email);
      }
```            

- 조회한 날짜에 해당하는 요일의 목록만 걸러서 반환
  
```xml
  LocalDate date = LocalDate.today();         
  //오늘 날짜를 요일로 바꾸기
  DayOfWeek dayOfWeek = date.getDayOfWeek();
  
  public boolean dividedDayOfWeek (DayOfWeek dayOfWeek, String intakeFrequency) {
  //섭취 식품 목록에서 intakeFrequency를 가져와서 대문자로 바꾸고, 조회한 요일을 3글자로 자르기
    String upperIntakeFrequency = intakeFrequency.toUpperCase();
  
    String currentDayString = dayOfWeek.name().substring(0, 3);
    //intakeFrequency에 조회한 요일이 속해 있다면 true 반환
    if(upperIntakeFrequency.contains(currentDayString)){
        return true;
    }
    return false;
  }
``` 


    
