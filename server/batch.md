## Batch Job

- 대규모 데이터를 처리하거나 반복적인 작업을 자동화하기 위해 일정한 시점에 실행되는 작업
- 실시간으로 처리되지 않아도 되는 작업들을 한꺼번에 모아서 (일괄처리) 일정한 시점이나 주기에 맞춰(스케줄링) 처리하는 방식
- 데이터 백업, 주기적인 이자 지급 등

#### - 작업 수행

`Spring batch` : java 기반의 대규모 데이터 처리 프레임워크, 트랜잭션 관리, 병렬처리, 재시도 로직 등의 고급기능 제공
  
#### - 스케줄링

`Quartz` : java 기반의 작업 스케줄러, 정교한 스케줄링 작업 관리<br/><br/>
`Crontab` : unix / linux 시스템에서 주기적인 작업을 자동으로 실행할 수 있도록 설정하는 도구 <br/> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;특정 시간에 스크립트를 실행하거나 명령을 자동으로 실행하는 데 사용

|Spring batch + Quartz|Python + Crontab|
|------|---|
|대규모 데이터 처리와 복잡한 스케줄링 요구사항 충족|단순하고 직관적인 작업 스케줄링에 유리|
|고급기능이 필요하거나 재시도, 트랜잭션 관리가 중요한 경우에 적합|기존 리소스 최대한 활용 (특별한 라이브러리, 추가 소프트웨어 필요 x)|

<br>

#### - ex) 프로젝트에서 사용한 Python + Crontab
- 현재 functional_food_detail 테이블에 공공데이터 저장되어있는 상태, 약 39,000개
- 공공데이터가 업데이트 될 때 프로젝트 DB 테이블에도 반영해야 함
- 식품의약품안전처_건강기능식품정보 OpenAPI에서 데이터 가져와서 DB에 저장하는 <br>
  비교적 단순한 작업이므로 Python 스크립트를 작성하고 Crontab에 특정 시간에 실행되도록 설정함

 ```
  * Crontab 설정
    - 프로그램 실행 중인 ubuntu 서버에 crontab 설정
    - crontab -e  크론탭 설정 파일을 열어 편집 ctrl + o -> enter -> ctrl + x 
    - crontab -l  크론탭에 설정된 모든 크론 작업 목록을 표시

    - 크론 표현식(Cron Expression) : 스케줄링 작업의 실행 시점을 정의하는 문자열

       * * * * *
       | | | | |
       | | | | └──요일 (0 - 7) (0 또는 7 = 일요일, 1 = 월요일, ..., 6 = 토요일) 
       | | | └────월 (1 - 12) 
       | | └──────일 (1 - 31) 
       | └────────시 (0 - 23) 
       └──────────분 (0 - 59)    

      * : 모든 값 (예: 분 필드에 *을 넣으면 매분마다 실행)
      , : 값 목록(예: 1,2,3는 1, 2, 3을 의미)
      - : 범위 (예: 1-5는 1에서 5까지를 의미) 
      / : 간격 (예: */15는 15분 간격을 의미)
      L : 마지막 (예: 일 필드에서 L은 마지막 날을 의미)
      -> Quartz는 초(second)를 포함해 6개의 필드로 확장된 표현식을 사용 -> 정교한 스케줄링        

      ex) 0 0 * * * 매일 자정에 실행  

          0 0 * * * /usr/bin/python3 /home/ubuntu/purewithme/functional_food_update.py
          -> 크론 작업에서 Python 스크립트를 실행하려면 Python 인터프리터를 명시적으로 지정해야 함
             (Python이 설치된 경로를 정확히 명시)
```
```
 * Python 설치
   - Python 인터프리터는 전역 환경 or 가상 환경에 설치 할 수 있다
   - 전역 환경(global environment)에 설치
     시스템 전체에서 사용할 수 있지만, 다른 프로젝트들과 의존성 충돌 문제가 발생할 수 있음.
   - 가상 환경(virtual environment)에 설치
     각 프로젝트마다 독립된 환경을 제공하여 의존성 충돌을 방지함

   가상환경 생성 (python3 -m venv - 명령어를 실행한 위치에 가상환경 생성 ~/myenv )
        |
   가상환경 활성화 (source /home/ubuntu/myenv/bin/activate)
        |
   Crontab 설정 전 Python 파일 실행되는지 확인
   -> 


```

```
  - 로그 기록 남기기
      1. Crontab에 설정시
        05 15 * * * /bin/bash -c 'source /home/ubuntu/myenv/bin/activate && /home/ubuntu/myenv/bin/python /home/ubuntu/purewithme/functional_food_update.py >> /home/ubuntu/functional_food_update.log 2>&1'
            시간    /      셸 선택       /          가상환경 활성화           /        인터프리터 지정       /                  파이썬 파일 실행                  -> 실행결과를 log 파일에 기록

      2. Python 스크립트에 logging 모듈을 사용하여 설정시
        import logging 
        logging.basicConfig(filename='/home/ubuntu/functional_food_update.log', level=logging.INFO)
        logging.info (f"업데이트 날짜 : {today_date}, 업데이트 데이터 수 : {total_inserted}")
        -> 로그파일 없다면 생성하여 저장함
```


