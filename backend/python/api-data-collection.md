# python script 작성



### * api에서 가져온 데이터 db 저장하기 위해 파이썬에서 필요한 라이브러리

- requests : 요청을 보내 API에서 데이터를 가져옴
- BeautifulSoup : 가져온 xml 데이터를 파싱
- pandas : 데이터를 DataFrame 형태로 변환하여 조작, 분석
- psycopg2 : PostgreSQL 데이터베이스에 연결하고 데이터를 삽입

- 데이터베이스 정보, params 를 딕셔너리 형식으로 선언
   ```
     *딕셔너리(Dictionary)
       : 키(key)와 값(value) 쌍으로 데이터를 저장하는 자료형, 
         unordered(순서가 정해져 있지 않음)하고, mutable(변경 가능)한 특성
      
        -선언 : 딕셔너리 형태로 데이터 할당
		db_info = {
	           'dbname' : 'purewithme'
	        	...
	        }
        -저장 : 딕셔너리 내부의 데이터를 실제로 저장하는 행위, 값을 변경하거나 추가
                params['pageNo'] = str(page)
    ```


- 데이터 가져오기 함수
    ```
	def fetch_data(page):
	    params['pageNo'] = str(page) -params에 값 추가하여
	    response = requests.get(url, params=params) -url과 params dictionary 가지고 데이터 가져옴 
	    return response
    ```
- 응답데이터에서 totalCount를 가져와서 페이지 계산 ex) 250개 데이터를 1페이지에 100개씩 가져온다면
                    
   ```
   total_pages = (total_count // 100) + (1 if total_count % 100 > 0 else 0)
                    몫 연산자 : 2      +     나머지 연산자 : 1            = 총 3페이지 필요
   ```

- range를 start_page ~ total_pages +1로 설정하고 for문 돌림 

- 각 페이지의 데이터 항목 추출하고, 메인 기능 키워드 매칭하여 설정

- api에서 가져온 데이터와 매칭한 키워드를 data = [] (데이터 리스트)에 추가

- data를 df로 변환하고 insert_record 메서드 실행
  ```
  df = dataframe : pandas 라이브러리가 제공하는 데이터 구조,
                     데이터필터링, 그룹화, 변환, 정렬,집계 등의 복잡한 작업을 간편하게 할 수 있도록 한다.
      ex) batch = df.iloc[start:end] dataframe에서 일부분을 잘라내는 구문 
          iloc (index-location) 인덱스 기반으로 행과 열에 접근

  insert_record : 파라미터로 df(dataframe), batch_size (1000개) 받아서 db에 배치로 삽입하는 메서드
       psycopg2를 이용하여 db와 연결하고 df 배치사이즈로 슬라이싱하여 cursor 객체로 쿼리 실행
         -cursor.executemany - 동일한 SQL 쿼리를 여러 번 실행하는 데 사용, 주로 다수의 데이터를 한 번에 삽입하거나 업데이트할 때 유용
         -cursor.execute - 단일 SQL 쿼리를 실행하는 데 사용
         -query - 이때 중복 레코드 방지하고 새로 저장된 데이터만 업데이트 하기위해 처음에 on conflict do nothing 절 사용했으나 데이터가 없어도 sequence가 증가되는 문제 
                   -> 이를 막기 위해 WHERE NOT EXISTS 절을 사용하고 statement_no를 unique key로 설정하여 db에 statement_no 같은 값 없는 경우에만 업데이트 되도록 함. 
  ```

 - `if __name__ == "__main__"` : 해당 파이썬 파일이 직접 실행 될 때 실행되는 코드<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                          import 해서 사용시 실행되지 않음 - module로 사용 시 불필요한 코드실행을 막기 위함       
- `__name__` :  파이썬의 내장변수, 어떻게 실행되고 있는지에 따라 다른 값을 가짐 
    - 직접실행시 __name__ == "__main__"
    - 다른 파일에서 import 시 __name__ =="파이썬 파일이름"









