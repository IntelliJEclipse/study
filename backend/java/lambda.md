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

## 메서드 참조 (Method References) A::B
- 람다식을 더 간단하게 표현할 수 있는 구문
- 이미 정의된 메서드를 참조하여 사용하는 방식
- 코드가 간단하고 명확할 때 사용
- 메서드를 참조해서 매개 변수의 정보 및 리턴 타입을 알아내어, 람다식에서 불필요한 매개 변수를 제거
- 람다식이 메서드를 실행하는 방식이라면, 메서드 참조는 이 메서드를 어디에서 사용하겠다는 선언에 가까움

#### * 메서드 참조 종류
|정적 메서드 참조 (ClassName::methodName)|
|---------|
|클래스 안에 작성된 사용자 정의 메서드나, 자바의 표준 라이브러리에 포함된 정적 메서드를 모두 포함하여 참조|
|람다식 : list.forEach(item -> System.out.println(item)); item을 매개변수로 하는 람다식<br/> 메서드참조 : list.forEach(System.out::println);  `System.out 객체의 println 메서드를 참조` |

|인스턴스 메서드 참조|
|---------|
|`특정 객체 메서드` : 특정 객체의 메서드를 참조하여 호출 (instance::methodName) <br/> 람다식 : list.forEach(item -> prefix.concat(item)); <br/> 메서드참조 : list.forEach(prefix::concat);  `String prifix = "fruit : ";  prefix객체의 concat() 메서드 참조`|
|`임의 객체 메서드` : 객체가 아닌 클래스의 메서드를 호출 ClassName::instanceMethod <br/> 람다식 : list.forEach(item -> item.toUpperCase()); <br/> 메서드참조 : list.forEach(String::toUpperCase);  `String prifix = "fruit : ";  String 클래스의 toUpperCase 메서드 참조`|

|생성자 참조 (ClassName::new)|
|---------|
|-클래스의 생성자를 참조하여 객체를 생성<br/> -기존 객체를 수정하지 않고 새로운 객체를 만들어야 할 경우<br/> -깊은 복사가 필요할 때, 독립적인 객체 복사<br/> -객체 변환 혹은 타입 변환이 필요할 때|
|람다식 : list.stream().map(item -> new String(item)).collect(Collectors.toList());<br/> 메서드참조 : list.stream().map(String::new).collect(Collectors.toList());|

```xml
ex) 특정 객체 메서드 참조
 FunctionalFoodIntakeEntity functionalFoodIntakeEntity = new FunctionalFoodIntakeEntity();
 functionalFoodEntity.ifPresent(functionalFoodIntakeEntity::setFunctionalFood);

 여기서 this는 이 클래스, 즉 functionalFoodService를 뜻한다.
 .forEach(this::deleteFunctionalFoodRecord);
```

```xml
ex) 생성자 참조 - 주로 스트림과 컬렉션에서 객체를 변환할 때 사용
                 
List<String> list = Arrays.asList("Apple", "Banana", "Cherry");

// String::new을 사용하여 새로운 String 객체를 생성
List<String> newList = list.stream().map(String::new).collect(Collectors.toList());

System.out.println(newList);  // [Apple, Banana, Cherry]

```

*String과 같은 불변객체는 한번 생성된 후 변경할 수 없기 때문에 객체 수정하기 위해 새로운 객체를 만들어야 함.

```xml
List<String> list = Arrays.asList("사과", "바나나", "딸기");

// 얕은 복사 (단순 참조 복사)
List<String> shallowCopy = new ArrayList<>(list);

// 깊은 복사 (새로운 String 객체 생성)
List<String> deepCopy = list.stream().map(String::new).collect(Collectors.toList());
```
