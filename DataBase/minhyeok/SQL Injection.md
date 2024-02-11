# SQL Injection 이란?

    악의적인 사용자가 보안상 취약점을 이용하여 이득을 얻기 위해 SQL 쿼리문을 조작하여 DB가 비정상적인 동작을 하도록 유도하는 행위이다. 공격이 비교적 쉬우나, 큰 피해를 입힐 수 있어 많은 주의가 필요하다.

# 종류

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F9958373C5C8890FA03" width="800" height="400">

<br>

- 입력값에 대한 검증이 없을 때 발생

- 특정 SQL 구문을 주입하여 WHERE 절을 참으로 만들고, 뒤의 구문을 주석 처리하여 모든 정보를 조회

- 이로 인해 가장 먼저 만들어진 계정으로 로그인에 성공할수도 있다. (관리자 계정) => 추가적인 피해


## Union based SQL Injection

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99BD4C3C5C8890FA0A" width="800" height="400">

<br>

- Union 키워드를 사용하여 정상적인 쿼리문에 추가적인 쿼리문을 주입

- 이를 통해 원하는 쿼리문을 실행할 수 있게 된다. 

- 두가지 조건이 필요 하나는 Union 하는 두 테이블의 컬럼 수가 같아야 하고, 데이터 형이 같아야 한다.

## Blind SQL Injection

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99525F3C5C8890F90E" width="800" height="400">

<br>

- 데이터베이스로부터 특정한 값이나 데이터를 전달받지 않고, 단순히 참과 거짓의 정보만 알 수 있을 때 사용

- Boolean based는 로그인 성공과 실패 메시지를 이용하여 DB의 테이블 정보 등을 추출해 낼 수 있다.

## Time based

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99CAFB395C88914513" width="800" height="400">

<br>

- 서버의 응답 시간을 이용하여 데이터베이스의 정보를 유추하는 기법.

## Stored Procedure SQL Injection

- 저장 프로시저를 이용하여 SQL Injection을 수행하는 방법

- 특히 MS-SQL의 xp_cmdshell과 같은 특정 저장 프로시저를 이용하여 윈도우 명령어를 사용할 수 있다.

## Mass SQL Injection

- 한 번의 공격으로 다량의 데이터베이스를 조작하여 큰 피해를 입히는 방법

- 주로 MS-SQL을 사용하는 ASP 기반 웹 애플리케이션에서 많이 사용되며, 쿼리문은 HEX 인코딩 방식으로 인코딩하여 공격

<br>

# 대응방법

## 입력 값에 대한 검증

- 사용자로부터 입력 받은 값에 대한 검증은 필수

- 이때 화이트리스트 기반의 검증이 필요하며, 블랙리스트 기반의 검증은 빠진 항목 하나로 인해 공격에 성공할 위험

- 공격 키워드와는 의미 없는 단어로 치환

## Prepared Statement 구문 사용

- 사용자의 입력 값을 문자열로 인식하게 하여 전체 쿼리문이 공격자의 의도대로 작동하지 않도록 하는 방법

- DBMS가 미리 컴파일하여 실행하지 않고 대기하며, 사용자의 입력은 이미 의미 없는 단순 문자열로 인식

## Error Message 노출 금지

- 데이터베이스 에러 발생 시, 에러가 발생한 쿼리문과 함께 에러에 관한 내용을 반환하면, 이를 통해 테이블명 및 컬럼명 그리고 쿼리문이 노출될 수 있다. 

- 따라서, 사용자에게 보여줄 수 있는 페이지를 제작하거나 메시지박스를 띄우는 등의 방법으로 에러 메시지의 노출을 막아야 한다.

## 웹 방화벽 사용

- 웹 공격 방어에 특화된 웹 방화벽의 사용은 효과적인 보안 방법 중 하나

- 웹 방화벽은 소프트웨어 형, 하드웨어 형, 프록시 형 등의 종류가 있으며, 각각의 방법은 서버에 직접 설치하거나, 네트워크 상에서 서버 앞 단에 하드웨어 장비로 구성하거나, DNS 서버 주소를 웹 방화벽으로 바꾸는 등의 방식으로 운용된다.

