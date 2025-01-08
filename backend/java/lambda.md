## 람다식 (Lambda Expression) ()->{}
- 간단한 함수를 코드에 인라인으로 작성할 수 있게 해주는 기능
- 자바 8부터 지원, 익명 클래스를 줄이기 위해 사용
- 복잡하고 다양한 로직 구현가능
  
```xml
  BloodSugarAlarm alarm = alarmRepository.findById(alarmId).orElseThrow(() -> new IllegalArgumentException("Invalid alarm ID"));
```

```xml
   .forEach( foodRecord -> {
                foodRecordRepository.save(foodRecord);
                foodRecordList.add(foodRecord);
            });
```

## 메서드 참조 (Method References)
- 람다식을 더 간단하게 표현할 수 있는 구문
- 이미 정의된 메서드를 참조하여 사용하는 방식
- 코드가 간단하고 명확할 때 사용
- 메서드를 참조해서 매개 변수의 정보 및 리턴 타입을 알아내어, 람다식에서 불필요한 매개 변수를 제거

#### * 메서드 참조 종류
    - 정적 메서드 참조 - 클래스::메서드 클래스 안에 작성된 사용자 정의 메서드나, 자바의 표준 라이브러리에 포함된 정적 메서드를 모두 포함하여 참조
    람다식 : list.forEach(item -> System.out.println(item)); item을 매개변수로 하는 람다식
    메서드참조 : list.forEach(System.out::println); System.out 객체의 println 메서드를 참조 ClassName::methodName

-인스턴스 메서드 참조 
     String prefix = "Fruit: ";
    특정 객체 메서드 : 특정 객체의 메서드를 참조하여 호출 instance::methodName
list.forEach(prefix::concat);
    람다식 : list.forEach(item -> item -> prefix.concat(item));

     임의객체의 메서드 : 객체가 아닌 클래스의 메서드를 호출 ClassName::instanceMethod
 list.forEach(String::toUpperCase);
     람다식 : list.forEach(item -> item.toUpperCase());

-생성자 참조 : 클래스의 생성자를 참조하여 객체를 생성 ClassName::new
list.stream().map(String::new).collect(Collectors.toList());
     람다식 : list.stream().map(item -> new String(item)).collect(Collectors.toList());


ex) 
인스턴스 메서드 참조
 FunctionalFoodIntakeEntity functionalFoodIntakeEntity = new FunctionalFoodIntakeEntity();
                        functionalFoodEntity.ifPresent(functionalFoodIntakeEntity::setFunctionalFood);

특정 객체 메서드 참조 - 여기서 this는 이 클래스, 즉 functionalFoodService를 뜻한다. 
.forEach(this::deleteFunctionalFoodRecord);
