---
layout: post
title: "정형 데이터 (structured data)"
category: data
tags: [데이터]
comments: true
---

------



##  목차

* [데이터베이스 개요](https://gungadinn.github.io/data/2019/07/08/structured-Data/#데이터베이스-개요)
* [RDBMS](https://gungadinn.github.io/data/2019/07/08/structured-Data/#RDBMS)
* [SQL](https://gungadinn.github.io/data/2019/07/08/structured-Data/#SQL)
* [SQLite](https://gungadinn.github.io/data/2019/07/08/structured-Data/#SQLite)

<br>

##  데이터

---

* 정형 데이터 (structured data)
* 반정형 데이터 (semi structured data) : JSON, XML
* 비정형 데이터 (unstructured data)

<br>

##  데이터베이스 개요

---

* 80% (unstructured) vs 20% (structured)

* Big Data = strcutured data + unstructured data (what companies don't know how to exploit yet)

* Data : raw facts, now context, just numbers and text

* Information: processed data, data with context, value-added data => make decisions

* 

  | database                                                     | data                                              | information                                     |
  | ------------------------------------------------------------ | ------------------------------------------------- | ----------------------------------------------- |
  | collection of data organized in a manner that alllows access, retrieval, and use of that data | collections of unprocessed items (text, number, ) | processed data (document, audio, images, video) |

<br>

### 데이터베이스

* a collection of interrelated data
* a set of intergrated stored, shared and operational data

<br>

###  데이터베이스 조건

* 통합 (Integrated)
  * 동일한 데이터가 중복되지 않도록 구성
  * 최소한의 중복 또는 통제된 중복만 허용
* 저장 (Stored)
  * 컴퓨터로 접근 가능한 물리적 저장 매체
* 공유 (Shared)
  * 공동으로 소유하고 유지하며 이용하는 데이터
* 운영 (Operational)
  * 존재 목적이나 기능 수행에 꼭 필요한 데이터 집합

<br>

### 데이터베이스 특징

* 실시간 접근성 (Real-time Accessibility)
  * 데이터들 간의 밀접한 관계로 연결
  * 중복 데이터를 배제하도록 지양
  * 사용자의 어떤 요구에도 즉각 응답
* 계속적인 변화 (Continuous evolution)
  * 현실 세계의 상태를 정확히 반영
  * 동적으로 삽입/삭제/수정하여 현재의 데이터 유지
* 동시 공유 가능 (Concurrent sharing)
  * 여러 사용자들이 동시에 이용
  * 같은 시간에 같은 데이터에 접근하여 이용
* 내용에 의한 참조 가능 (Content reference)
  * 저장된 주소나 위치에 의해서 참조하지 않고 사용자가 요구하는 데이터의 내용/값에 따라 참조

<br>

### DBMS

* a collection of programs
* manage the database structure
* controls access to the data stored in the data

<br>

### DBMS의 역할

* 데이터베이스들 수정 (create databases)
* 데이터 삽입, 수정, 삭제 (insert, update and delete data)
* 데이터들 정렬 및 쿼리 (sort and query data)
* create from and report

<br>

### DBMS 역할과 장점

* Improved data sharing
* Improved data security
* Minimized data inconsistency
* Improved data access
* Improved decision making
* Increased end-user productivity
* Reduce application development data

<br>

### DBMS 종류

* 계층형 모델 (Hierarchical Datase Model)
  * 트리 형태로 표현 (1:N)
  * 객체를 노드, 객체 집합들 사이의 관계를 링크로 표현
* 네트워크형 모델 (Network Database Model)
  * 그래프 형태로 표현 (N:M)
  * 각 개체가 여러 관계를 가질 수 있음
* __관계 데이터 모델__ (Relational Database Model)
  * 객체를 테이블, 객체 집합들 사이들 관계를 공통 속성으로 연결
* __객체 관계 모델__ (Object-Relational Database Model)
  * 속성-관계
  * 객체-관계

<br>

## RDBMS

---

### 왜 RDBMS를 사용하는가?

* 데이터 안전성 (Data Safety)
* 동시 접근성 (Concurrent Access)
* __장애 허용성__ (Fault Tolerance)
* 데이터 무결성 (Data Integrity)
* 데이터 확장성 (Scalability)
* 데이터 보고서 (Reporting)

<br>

### RDBMS Concepts

* Relation / table
* Tuple / Row or Record
* Attribute / Column or Field
* Cardinality / Number of Rows
* Degree / Number of Columns
* Domain / Pool of legal values
* Primary key / Unique identifier

<br>

### 개체 관계 모델 (Entiti-Relationship-Model)

* 개체 (Entity)
  * 실 세계에 존재하는 분리된 실체 하나를 표현, 일반적으로 명사 하나에 해당
* 관계 (Relationship)
  * 개체들 사이에 존재하는 연관이나 연결, 일반적으로 동사에 해당
  * 최소 대응수(minimun cardinality)와 최대 대응수(maximun cardinality)로 구성
* 속성 (Attibute)
  * 개체의 성질, 분류, 식별, 수량, 상태 등을 나타내는 세부 항목
  * 관계 또한 속성을 보유할 수 있음
* 기본키 (Primary key)
  * 모든 개체를 고유하고 식별할 수 있는 속성

<br>

## SQL

---

### SQL

*  Structured Query Language
  * RDBMS의 데이터를 관리하기 위해 만들어진 언어 (대부분 ISO 표준을 따름)
  * 자료의 검색과 관리, 데이터베이스 스키마 생성과 수정, 데이터베이스 객체 접근 조정 관리 등을 고안

<br>

### SQL 명령

* 데이터 정의 언어 DDL (Data Definition Language)
  * CREATE, DROP, ALTER, TRUNCATE , ...
* 데이터 조작 언어 DML (Data Manipulation Language)
  * INSERT, UPDATE, DELETE, SELECT ,...
* 데이터 제어 언어 DCL (Data Control Language)
  * GRANT, REVOKE, ..

<br>

### SQL 구문

* 문(Statement), 절(Clause), 식(Expression), 술어(Predicate)

<br>

### Data Type

* Boolean (BOOLEAN)

  * TRUE나 FALSE 표현

* Character (CHAR, VARCHAR

  * CHAR : 고정    ex) 주민등록번호
  * VARCHAR : 가변   ex) 댓글

  * 이름이나 사람, 도시들과 같은 column을 표현

* Exact numeric (NUMERIC, DECIMAL, INTEGER, SMALLINT, BIGINT)

  * 숫자 표현

* Approximate numeric (REAL, FLOAT, DOUBLE)

* Datetime (DATE, TIME, TIMESTAMP)

  * time of a day
  * datetime fields(시간, 분, 초) 들을 포함한다.
  * Syntax : TIME[( precision )]
  * Precision : seconds value, default is 0 / 3 means miliseconds / 6 means microseconds

<br>

## SQLite

---

```python
# sqlite3 import
import sqlite3

# 버전 출력
print(sqlite3.version)
print(sqlite3.sqlite_version)

# 데이터베이스 연결(생성)
conn = sqlite3.conntect(':memory:')
# cursor 생성 (해당 database의 connection으로부터 인스턴스 생성)
cur = conn.cursor()
```

<br>

```python
dir(conn) 
# conn 에도 cursor가 있지만 임시로 생성하기 때문에 cur = conn.cursor()처럼 cursor를 받아서 쓰는 것이 좋다

dir(cur)
# execute - sql문 실행
# executemany - 하나의 sql문을 sequential 여러개 입력해야 할 때
# executescript - 여러 개의 sql문 실행

# fetchall - 리스트의 형태로 결과를 전부 보여준다
# fetchone - 결과를 하나만 가져온다
# fetchmany(size) - 결과를 내가 지정한 갯수만큼만 가져온다
```

<br>

```python
# 테이블 생성
cur.execute("create table people (name_last, age)")

who = "아라곤"
age = 130

# qmark style
cur.execute("insert into people values(?, ?)", (who, age))

# named style
cur.execute("select * from people where name_last=:who and age=:age", {"who":who, "age":age})

print(cur.fetchone())
# output : ('아라곤', 130)
```

* qmark style : 파라미터를 ?로 넘기는 것
  * 데이터 구조가 복잡해지거나 변수명이 섞이거나 관계를 가지면 힘들어진다
* named style : named placeholders
  * key-value  쌍으로 넘기면 되기 때문에 변수명만 제대로 넘겨주면 쿼리를 수정 할 필요가 없다. 데이터 값만 바꾸면 된다.

<br>

```python
# 데이터베이스 닫기
conn.close()
```

<br>

### 1) executemany

#### (1) qmark style

```python
sql = "insert into people values (?,?)"
curData = [('A',1), ('B',2), ('C',3)]
cur.executemany(sql, curData)
```

<br>

#### (2) named style

```python
dataDict = [{"who":"E", "age":4},{"who":"D", "age":5},{"who":"F", "age":6}]
cur.executemany("INSERT INTO people VALUES(:who, :age)", dataDict)
cur.execute("SELECT * FROM people")
cur.fetchall()
# output : [('아라곤', 130), ('A', 1), ('B', 2), ('C', 3), ('E', 4), ('D', 5), ('F', 6)]
```

<br>

### 2) executescript

`commit` 문을 먼저 실행 한 다음 매개변수로 가져온 sql 스크립트를 실행한다.

```python
cur.executescript("""
    create table person (
        first_name text pirmary key,
        last_name text not null
    );
    
    insert into person values ('name', 'kim');
""")

cur.execute('select * from person')
print(cur.fetchall())
# output : [('name', 'kim')]
```

<br>

### 