# SQL 쿼리문

- CURRENT_TIMESTAMP()로 insert되는 purchaseDate에 대해 자동으로 add 1day 되는 expireDate 설정
```sql
ALTER TABLE tblPurchasedEmoticon ADD COLUMN expireDate DATETIME GENERATED ALWAYS AS (DATE_ADD(purchaseDate, INTERVAL 1 DAY)) STORED;
```
