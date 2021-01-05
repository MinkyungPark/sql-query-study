# sql-query-study
sql 쿼리문 정리 
[]는 생략가능

<br>


## DML(Data Manipulation Language)
### 1. SELECT
- select {컬럼명} from {테이블명} [where 조건] [order by 컬럼명 ASC(기본값,오름차순)/DESC] [group by 컬럼] [LIMIT 개수];
### 2. INSERT
- insert into {테이블명}[(c1,c2)] values{v1, v2}; <br>
: [c1,c2..] 컬럼 생략시, 테이블명에 있는 컬럼명의 순서에 맞게 값 입력 <br>
- insert into table(col1, col2) values(v1,v2);
### 3. UPDATE
- update {테이블명} set 컬럼=바꿀값,.. [where 조건];
### 4. DELETE

---

### WHERE 조합
- AND, OR, NOT, IN <br>
: where col1 in('col1_value1','col1_value2'); ()안의 값이 하나 이상 일치하면 조건이 맞는 것 <br>
: where not col1 in('col1_value1'); col1_value1을 제외한 정보 <br>

### DISTINCT
- DISTINCT 중복 제거 값 얻기 <br>
: select distinct col1, col2 from table; <br>
distinct이후 모든 컬럼(2개이상)에 대해 중복 제거 <br>


### JOIN, UNION
#### UNION
- UION 사용시 필드의 이름이 같아야 하며 같지 않으면 AS를 통해 같게 만들어준다. 대응되는 각 필드의 타입이 같아야한다.
- UNION 중복되지 않는 유일한 값
(select col1, col2 from table) <br>
union <br>
(select col1, col2 from table2 where col1 = 'a')
- UNION ALL 중복되더라도 모든 값 추출
(select col1, col2 from table) <br>
union all <br>
(select col1, col2 from table2 where col1 = 'a') <br>
order by col1 // order by 끝에 추가 가능
#### JOIN
- 그냥 where 문을 써서 외래키가 같은 것. 해서 가능
SELECT I.NAME AS NAME, I.DATETIME AS DATETIME <br>
FROM ANIMAL_INS AS I, ANIMAL_OUTS AS O <br>
WHERE I.ANIMAL_ID = O.ANIMAL_ID <br>
- 1. inner 조인
select gg.id, gg.name, s.title <br>
from girl_group as gg <br>
join song as s <br>
on s.id = gg.hit_song_id; <br>
- 2. left outer, right outer 조인 
from절에 오는 테이블이 기준(left면 left쪽이 되는..) <br>
select gg.id, gg.name, s.title <br>
from girl_group as gg <br>
left join song as s <br>
on s.id = gg.hit_song_id; <br>
- 3. outer 조인
mysql에서는 지원하지 않으나 유사하게 구현할 수 있음 <br>
SELECT s._id, s.title, gg.name FROM girl_group AS  <br> gg JOIN song AS s ON s._id <> gg.hit_song_id;  <br>
- 4. cross 조인(카티전 조인)
- 5. 셀프조인

# 집합
- 곱집합
select A.id, A.name, B.id, B.name <br>
from TableA as A, TableB as B
- 합집합 UNION
- 교집합 INNER JOIN
- 차집합 
left, right join 한뒤 is null <br>
where not exists (subQuery) <br>
where col not in (subQuery) <br>
EX <br>
A - B <br>
1) <br>
select A.id, A.name<br>
from tableA as A left join tableB as B<br>
on A.id = B.id<br>
where B.id is null<br>
2) <br>
select A.id, A.name from tableA as A<br>
where not exists(<br>
&nbsp;&nbsp;&nbsp;select distict B.id from tableB as B where B.id = A.id<br>
)<br>
3)<br>
select A.id, A.name from tableA as A<br>
where A.id not in (<br>
&nbsp;&nbsp;&nbsp;select distinct tableB.id from tableB as B<br>
)<br>

## LIKE
- SELECT * FROM member WHERE name LIKE '홍%;
- SELECT * FROM member WHERE name LIKE '% 길동;
- SELECT * FROM member WHERE name LIKE '% 길동%;

## IFNULL
## WITH
- 서브쿼리를 내마음대로 사용할 수 있다.
- 인라인뷰를 재사용 할 수 있다.

### GROUP 함수 + 집계함수
- GROUP BY + HAVING <br>
: 집계함수의 조건은 having 절에서만.
- SUM, MAX, MIN, COUNT, AVG <br>
: 위의 집계함수 없이 group by 를 사용할 경우 distinct와 비슷해진다.
: max, min의 경우 출력할 컬럼과 조건의 컬럼이 일치할떄만 사용이 가능하다? ex 59415번 <br>
: select count(*) from table; 모든 컬럼의 수 <br>
: count와 group by 같이 <br>
: select name, count(name) from table group by name having count(name) >= 2; <br>
: select name, count(*) from table group by name having count(name) >= 2; <br>

### DATETIME 날짜데이터 다루기
- 일부만 추출 <br>
: YEAR(), MONTH(), DAY(), HOUR(), MINUTE(), SECOND()

---

<br>

## 프로시저, 서브쿼리

### 변수 사용
- DECLARE @name varchar(20); // 변수 선언
- SET @name = 'Amy'; // 선언과 초기화
- 대입 연산자 :=

### 서브쿼리
- 1. select 절에서
- 2. from 절에서
- 3. where 절에서

---

<br>

## DDL(Data Definition Language)
### 1. CREATE
- create table {테이블명}{{컬럼명}{컬럼타입}};
### 2. DROP
- drop table {테이블명}
### 3. ALTER
- alter table {테이블명} add {컬럼명}{컬럼타입}{조건(디폴트값, not null 등)}; <br>
- alter table {테이블명} modify {컬럼명}{컬럼타입}{조건}; <br>
- alter table {테이블명} drop {컬럼명}; <br>
### 4. TRUNCATE
### etc
- 이름변경 rename table {원래이름} to {바꿀이름}; <br>

<br>

## DCL(Data Control Language)
### 1. GRANT
### 2. REVOKE

<br>
<br>

## *참고(MySQL)
- MySQL 접속 <br>
: mysql -u root -p (dbname) <br>
- 비번 변경 <br>
: mysqladmin -u root password 새로운비밀번호 <br>
- 구조보기 <br>
: desc 테이블, explain 테이블, show create table 테이블명 <br>