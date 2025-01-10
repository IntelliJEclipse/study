## 참조 무결성 (Referential Integrity)
 외래 키(FK) 관계가 올바르고 일관되게 유지되도록 보장 <br> 
&nbsp;&nbsp;&nbsp;=> 외래 키가 참조하는 데이터가 반드시 존재해야 함 
- PK (Primary Key) : 기본 키, 각 레코드를 고유하게 식별, 중복 허용 X, null X 
- FK (Foreign Key) : 외래 키, 다른 테이블의 기본키를 참조하는 열, 테이블 간 연관관계 맺을 수 있음

#### - 참조 무결성을 보장하는 작업
- Cascade : 참조된 데이터가 삭제되거나 업데이트 될 때 이를 참조하는 데이터도 자동으로 삭제되거나 업데이트 됨.
    ```
       ex) user 테이블의 사용자 id가 삭제되거나 업데이트 작성한 모든 게시글도 함께 삭제되거나 업데이트
           ALTER TABLE posts
           ADD CONSTRAINT fk_user_id
           FOREIGN KEY (userId)
           REFERENCES users(id)
           ON DELETE CASCADE
           ON UPDATE CASCADE;
    ```
           
- SetNull : 참조된 데이터가 삭제, 업데이트 될 때 외래키 값이 null로 설정 <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;참조된 데이터가 삭제되어도 참조하는 데이터는 남아 있으나, 외래 키 값은 무효화
  
    ```
       ex) user 테이블의 사용자가 삭제되면 작성한 게시글의 userId는 NULL로 설정되며, 
           게시글은 그대로 남아 있음
           ALTER TABLE posts
           ADD CONSTRAINT fk_user_id
           FOREIGN KEY (userId)
           REFERENCES users(id)
           ON DELETE SET NULL
           ON UPDATE SET NULL;
    ```
           
- Restrict : 참조된 데이터가 삭제, 업데이트 되지 않도록 함 = no action (default)

#### - 외래 키 제약만으로 해결할 수 없는 부분

- 외래 키는 기본적으로 상대 테이블의 기본 키 (PK) 또는 unique 제약이 있는 컬럼에만 설정 가능<br>
  (참조하는 값이 고유하고 일관되게 유지될 수 있어야 하기 때문에, 제약이 걸린 컬럼만을 참조)
- nickname과 같은 비식별자 필드는 외래 키로 사용될 수 없으며, 변경된 부분을 반영하려면 트리거와 같은 방법을 사용함
 

  `트리거` : 데이터베이스에서 특정 이벤트가 발생했을 때 자동으로 실행되는 작업, 트리거 함수를 호출 <br>
  `트리거 함수` : 보통 PL/pgSQL이나 다른 프로시저 언어로 작성<br>
  `프로시저 언어 (Procedural Language)` : 특정 작업을 수행하기 위한 단계를 정의하고, 순서대로 실행하는 방식 <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ex) SQL/PSM, Transact-SQL (T-SQL), PL/SQL, PL/pgSQL<br>
  
  ex) user 탈퇴 후 user가 작성한 글의 nickname을 "탈퇴한 회원"으로 변경하고 싶다면?

  |user|post|
  |----|----|
  |id (sequence, PK)|id (sequence, PK)|
  |nickname|userId (FK)|
  |phone|nickname|
  |...|title|
  |...|content|

  -> user에서 Id는 sequence로 post가 userId라는 이름으로 참조하고 있다 (FK로 설정) <br>
  -> ON DELETE SET NULL을 설정하여 user 삭제 시 post 테이블의 userId가 NULL로 바뀜 <br>
  -> nickname은 연관관계 없기에 trigger를 설정하여 "탈퇴한 회원"으로 update 해줌 <br>
  -> 트리거가 트리거 함수를 호출
     ```
     * 트리거 함수

     CREATE OR REPLACE FUNCTION update_post_on_user_delete()
     RETURNS TRIGGER AS $$
     BEGIN
        -- 삭제된 사용자의 userId를 기준으로 게시글을 찾아 nickname을 '탈퇴한회원'으로 변경
        UPDATE posts
        SET nickname = '탈퇴한회원'
        WHERE userId = OLD.userId;  -- OLD.userId는 삭제된 사용자의 userId를 참조
        RETURN NULL;  -- 반환할 값이 없으므로 NULL - AFTER DELETE 트리거는 기본적으로 삭제 작업이 완료된 후 추가 작업, 반환값 필요X
     END;
     $$ LANGUAGE plpgsql; --$$ 델리미터 : 함수 본문을 포함하는 텍스트 블록의 시작과 끝
                          --LANGUAGE plpgsql : PL/pgSQL 언어로 작성되었음을 지정
     
     ```

     ```
     * 트리거 정의
     
     CREATE TRIGGER update_post_nickname_on_user_delete
     AFTER DELETE ON users  -- users 테이블에서 행이 삭제된 후 실행
     FOR EACH ROW            -- 각 삭제된 행에 대해 트리거가 실행
     EXECUTE FUNCTION update_post_on_user_delete();  -- 위에서 정의한 트리거 함수 호출
     ```

     

 
