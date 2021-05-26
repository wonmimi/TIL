###  rownum을 이용한 페이징 
- ROW_NUMBER() OVER()  이용
```sql
SELECT 컬럼1, 컬럼2 ,ROW_NUMBER() 
OVER([PARTITION BY 그룹핑 컬럼] [ORDER BY 정렬 컬럼 ]) AS 별칭 
FROM 테이블
```
기존 레거시 쿼리에 에러가 생겨서 실적용 했던 경우
  * 사내 채용 시스템 지원자 목록 페이징 처리 

```sql
SELECT * FROM (
    SELECT ROW_NUMBER() OVER
        (ORDER BY seq) as rnum, seq, apply_pass_exne, apply_kind, apply_kind_sub, crypto_aes256.dec_aes (apply_nm) AS apply_nm,
           crypto_aes256.dec_aes (apply_nm_han) AS apply_nm_han, apply_idnum, apply_pass, crypto_aes256.dec_aes (apply_birth) AS
               apply_birth, apply_gender, crypto_aes256.dec_aes (apply_tel) AS apply_tel, , apply_sch_ele, apply_sch_ele_nm, ..컬럼 ... 
    FROM APPLY
      -- where  apply_kind = 'B'  and apply_kind_sub = '3'
    ORDER BY SEQ
) where rnum >= 5 and rnum <= 100;
```
#

- rownum 이용 
```sql
SELECT * FROM (
        SELECT ROWNUM as rnum, a.* FROM (
               SELECT
               seq, apply_pass_exne, apply_kind, apply_kind_sub, crypto_aes256.dec_aes (apply_nm) AS apply_nm,
           crypto_aes256.dec_aes (apply_nm_han) AS apply_nm_han, apply_idnum, apply_pass, crypto_aes256.dec_aes (apply_birth) AS
               apply_birth, apply_gender, crypto_aes256.dec_aes (apply_tel) AS apply_tel, crypto_aes256.dec_aes (apply_cell) AS apply_cell, apply_sch_ele,  apply_sch_ele_fin, apply_sch_mid, ... 컬럼  ...
               FROM APPLY
--                    where  apply_kind = 'B'  and apply_kind_sub = '3'
               ORDER BY SEQ
       ) a
    )
WHERE rnum >= 1 and rnum <=10;

```

[참고](https://programmer93.tistory.com/4)