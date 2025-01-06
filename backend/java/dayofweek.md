## DayOfWeek
LocalDate 객체의 메서드, 날짜의 요일을 DayOfWeek 열거형(enum)으로 반환 ( MONDAY ~ SUNDAY )

```xml
  LocalDate date = LocalDate.of(2024, 11, 13);         
  DayOfWeek dayOfWeek = date.getDayOfWeek();
  -> WEDNESDAY 반환
```
### ex) DayOfWeek method를 이용하여 건강기능식품 섭취 요일에 따라 record list 반환하기
   - 등록된 섭취 식품 목록 테이블 (FunctionalFoodIntakeList)에서 intakeFrequency 컬럼은 'Mon,Tue' 등의 , 로 구분된 string 형태로 저장된 상태
   - 섭취 식품 목록을 조회하는 날짜를 받아서 요일로 변환하여 변수에 담고 요일 비교 메서드를 만들기

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
   - 조회한 날짜에 해당하는 요일의 목록만 걸러서 반환
   


    
