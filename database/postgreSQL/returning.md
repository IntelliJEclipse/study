## RETURNING 절
- postgreSQL에서 INSERT, UPDATE, DELETE 작업 후에 특정 값을 반환하고 싶을 때 사용
- 쿼리 실행 후 반환된 값으로 결과 식별
- 다른 DB도 유사한 기능 제공 (RETURNING INTO, OUTPUT 등)

```
  INSERT INTO users (email, nickname) 
  VALUES ('abc@test.com', 'hihi') 
  ON CONFLICT (email) 
  DO UPDATE SET nickname = EXCLUDED.nickname 
  RETURNING 
    CASE
      WHEN users.email = EXCLUDED.email THEN 'update'
      ELSE 'insert'
    END AS operation;
```
