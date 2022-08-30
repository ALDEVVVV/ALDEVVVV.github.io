# Postgre ,MongoDB공부

RDBMS를 거의 대부분 사용



Document관련(Mongo) /
key-value



DB생성 -> Talbe생성 -> 그후 처리속도를 위하여 Index관리



테이블 생성

-create table 테이블명 (id int , name varchar(20));



컬럼 추가

-alter table 테이블명 add [컬럼명][타입][옵션]

Ex) alter table 사원 add 주소 varchar(100) not null default’’;



컬럼명 및 타입 변경

-alter table 테이블명 change [컬럼명][변경할컬럼명] varchar(10);

-alter table 테이블명 modify [컬럼명] varchar(20);



삭제

-Drop table 테이블명;



테이블명 변경

-Alter table 테이블명 rename [변경할 테이블명]

DBMS마다 컨버전이 되거나 안되거나 하는경우가 다름



Index관리(where조건절에 있는 많이 참조하는 컬럼들)

생성

-create index 인덱스명 on 테이블명(컬럼명,…);

-alter table 인덱스명 add index(컬럼명,…);



조회

-show index from 테이블명;



삭제

-       Alter table 테이블명 drop index 인덱스명;

-       Drop index 인덱스명 on 테이블명;(테이블명 없을 시 인덱스 전체삭제)



Data관리

삽입

           -insert into 테이블명 (컬럼명) values (값,…);

변경

           -update 테이블 set 컬럼명 = 값 where 조건절;

삭제

           -delete from 테이블명 where 조건절;

데이터 조회

           -select  from 테이블명 where 조건절;



인덱스 관리 시에 프로그램에서 어떤 SQL을 쓰고있는지

어떤 패턴의 SQL을 사용하는지를 생각하여 처리가능

Ex) 학생부 검색 시 누구를 검색할 때 이름으로 먼저검색하는 경우 , 학번(primary key)으로 먼저하면 다이렉트로 좀더
빠르고 적은 용량을 이용하여 검색 가능



Delete와 truncate의
차이

Delete From 사원 Where 성명 like ‘백%’;

Truncate From 사원 where 성명 like ‘백%’; // 사용하던 공간까지 반납

         - 해당 테이블의 데이터가 모두 삭제되지만 테이블 자체가 지워지는 것은 아님

         - 해당 테이블에 생성되어 있던 인덱스도 함께 truncate 됨



MongoDB – NOSQL

Key-value형식

Table –Collection

Document – Row

Field – Column

MongoDB명렁ㅇ

           생성

-       Use DB_NAME (1개 이상의 collection이 존재해야 리스트에 보임)

조회

-       Show DBS: DB리스트 확인

-       DB : 현재 사용중 database확인

-       DB.status() : DB상태확인

Collection관리

           생성

-       Db.createcollection(컬렉션명,[옵션])

n  옵션은 document타입

조회

-       Show collection

삭제

-       Db.컬렉션명.drop()

이름
변경

-       Db.OLD컬렉션명.renameCollection(“New컬렉션명”)

Index관리

           생성

-       Db.컬렉션명.createindex(document[,options])

조회

-       Db.컬렉셔명.getindexes()

삭제

-       Db.컬렉션명.dropindexes(도큐먼트)





MYDB생성 , SALE_DATA테이블생성 , 대량 데이터 import , SALE_DATA_NEW 복제 , 테이블용량확인(DELETE,TRUNCATE) , CSV파일로 export , Export한 데이터 저장



PATH 추가 (C:programfile\postgre\bin)



PSQL에서 

create database mydb; (생성)

Select * from pg_database; (db생성된 것 조회)



CMD창에서 디렉터리 이동후

Psql -d mydb -U postgres 01_tab_saledata.sql

PSQL에서 \c mydb (use mydb)



Import 하기

\copy sale_data from 'C:\postgresql_test\postgre\sales.csv'
delimiter ',' csv header;



새로운 테이블 생성

create table sale_data_new ( like sale_data);



내용 확인

select count(*) from sale_data_new;



삭제 후 복제

drop table sale_data_new;

create table sale_data_new as select * from sale_data;



확인

select count(*) from sale_data_new;



테이블 용량 확인

mydb=# insert into sale_data_new select * from sale_data;

INSERT 0 12738

mydb=# select count(*) from sale_data_new;

 count

-------

 25476

(1개 행)

Delete 하기(용량이
줄어들지 않음 데이터공간)

mydb=# delete from sale_data_new;

DELETE 25476

mydb=# select count(*) from sale_data_new;

 count

-------

     0

(1개 행)

mydb=# truncate table sale_data_new;

TRUNCATE TABLE

mydb=# select
pg_relation_size('sale_data_new');4

 pg_relation_size

------------------

                0

(1개 행)

Export하기

mydb=# \copy sale_data to
'C:\postgresql_test\postgre\emport_sale_data.csv' csv;

COPY 12738

Export한 데이터 저장

CMD에서 psql -d mydb
-U postgres -f 02_imp_saledata_new.sql

PGAdmin에서 확인

select * from sale_data;
