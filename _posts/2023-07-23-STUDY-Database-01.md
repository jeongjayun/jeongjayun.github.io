---
title: Database의 종류와 특징 🗂️
date: 2023-07-23 14:00:00 +/- TTTT
categories: [STUDY, Database]
tags: [db] # TAG names should always be lowercase
---

학원에서 Database는 RDB의 mySQL, Oracle 만 접해봤는데 찾아보니 Database의 종류가 많아도 너무 많다 🤯 !!
어떤 개발 목적에 어떤 Database 를 써야 하는지 궁금하여 간단하게 정리했다.

> **참고**
> 코딩애플님의 영상 [https://www.youtube.com/watch?v=ZVuHZ2Fjkl4](https://www.youtube.com/watch?v=ZVuHZ2Fjkl4)
{: .prompt-info}

## Database 의 종류 📂
1. key:value Database
    - Redis
1. Relation Database (RDB)
    - mySQL, Oracle, MariaDB, etc
1. Graph Database
1. Document Database
    - mongoDB, CouchDB
1. Column family Database
1. Search 
    - ElasticSearch, etc

## Database 의 특징들 🎯
### 1. Key : value Database

![image](https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/79ab54b0-ecc1-4c13-8817-4a58f04ed670)

1. 데이터를 key : value 로 저장한다.
1. key : value 로 간단하게 저장하기 때문에 서브용 Database로 사용한다.

#### <img src="https://img.shields.io/badge/redis-DC382D?style=for-the-badge&logo=redis&logoColor=white">
- 데이터를 key : value 로 저장한다.
- 데이터를 하드웨어가 아닌 **램**에 저장하기 때문에 속도가 빠르다.
(그래도 하드웨어에 백업은 한다.)
- **특징** : 메인 Database에서 자주 쓰는 정보들을 Redis에 복사해두고 사용자가 데이터를 꺼내야 할 경우 Redis 에서 꺼내어 제공한다.
- 보통 캐싱용 / 채팅을 위한 pub & sub 구현 / 영상 스트리밍 / 로그인 기록을 저장할 수 있다.

### 2. Relation Database, RDB

<div align="left">
<img src="https://img.shields.io/badge/mysql-4479A1?style=for-the-badge&logo=mysql&logoColor=white">
<img src="https://img.shields.io/badge/oracle-F80000?style=for-the-badge&logo=oracle&logoColor=white">
<img src="https://img.shields.io/badge/mariadb-003545?style=for-the-badge&logo=mariadb&logoColor=white">
<img src="https://img.shields.io/badge/microsoftsqlserver-CC2927?style=for-the-badge&logo=microsoftsqlserver&logoColor=white">
<img src="https://img.shields.io/badge/postgresql-4169E1?style=for-the-badge&logo=postgresql&logoColor=white">
</div>

<img width="1047" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/a44e7806-6d79-42f9-a55d-33cfdaf29baa">

> 출처 : [DB엔진](https://db-engines.com/en/ranking) , 2023년 7월 기준, 10위 안에 RDB가 7개나 들어있다.
{: .prompt-info}

1. 테이블 형식으로 데이터를 저장한다.
1. 데이터베이스 사용 점유율을 보면 관계형 데이터 베이스를 많이 쓴다.
1. 중복 데이터를 줄이기 위해 정규화 작업을 거친다.
1. 정확도가 중요한 작업 시에는 RDB가 좋다.


### 3. Graph Database

<div align="left">
<img src="https://img.shields.io/badge/neo4j-4581C3?style=for-the-badge&logo=neo4j&logoColor=white">
<img src="https://img.shields.io/badge/microsoftazure-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white">
</div>

1. Graph Query Language 를 이용한다.
- 비행기 노선, SNS 친구관계, 코로나 전염맵, 추천 서비스 등

### 4. Document Database

<div align="left">
<img src="https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=MongoDB&logoColor=white">
<img src="https://img.shields.io/badge/amazondynamodb-4053D6?style=for-the-badge&logo=amazondynamodb&logoColor=white">
<img src="https://img.shields.io/badge/apachecouchdb-E42528?style=for-the-badge&logo=apachecouchdb&logoColor=white">
</div>

1. Collection 안에 Document 들을 만들어 JSON 형태로 저장한다.
1. 데이터의 입출력이 간단하다.
1. 데이터의 중복제거를 하지 않는다. = 정규화 안함
1. 장점 : 분산처리를 매우 잘해준다.
1. 단점 : DB간 일관성이 떨어질 수 있다.
- SNS, 실시간 채팅, 게시판 온라인 게임 등


### 5. Column family Database

![image](https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/3049d09f-8428-4ad6-a9c9-ab239763838a)

- 테이블과 로우를 만들고 자유롭게 컬럼을 만든다.

### 6. Search형

<div align="left">
<img src="https://img.shields.io/badge/elasticsearch-005571?style=for-the-badge&logo=elasticsearch&logoColor=white">
</div>

1. index 보관에 특화되어 있다.
1. 검색이 중요한 사이트에서 주로 사용한다.
- 실시간 검색어, 추천검색어, 검색어 오타 교정 등 쉽게 만들 수 있음.
