# 조인이란?

    두 개 이상의 테이블에서 필요한 데이터를 가져와서 하나의 결과 집합으로 결합하는 연산입니다. 조인을 사용하면 여러 테이블에 분산된 데이터를 효율적으로 조회할 수 있습니다.

## 종류

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FluYd1%2FbtrbUXM4iJJ%2FqLt17TKyK9is4xdS1pK0ok%2Fimg.png" width="800" height="700">

<br>

### Inner Join(내부 조인)

    두 테이블의 교집합을 반환합니다. 즉, 조인 조건에 맞는 행만 결과로 반환하며, 조건에 맞지 않는 행은 제외됩니다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FJp2Vf%2FbtqE33quGjY%2F9kvKNZGxK6Z1eDT3VHYAek%2Fimg.png" width="800" height="300">

<br>

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdfAaJp%2FbtqE2tpEeo9%2FhAVVa3o3FP790ZzBiO5pCK%2Fimg.png" width="400" height="300">


### Left Outer Join(왼쪽 외부 조인)

    왼쪽 테이블의 모든 행과 오른쪽 테이블에서 조인 조건에 맞는 행을 반환합니다. 오른쪽 테이블에 매칭되는 행이 없는 경우에는 NULL 값을 반환합니다.

    SELECT A.ID, B.ID, A.NAME, B.NAME
    FROM Table A,  Table B
    WHERE A.ID = B.ID(+);

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Feb6dKq%2FbtqE42qPl9E%2FHmM4YZlkk2C8HO730kUzHK%2Fimg.png" width="400" height="300">


### Right Outer Join(오른쪽 외부 조인)

    오른쪽 테이블의 모든 행과 왼쪽 테이블에서 조인 조건에 맞는 행을 반환합니다. 왼쪽 테이블에 매칭되는 행이 없는 경우에는 NULL 값을 반환합니다.

    SELECT A.ID, B.ID, A.NAME, B.NAME
    FROM Table A,  Table B
    WHERE A.ID(+) = B.ID;

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbj8SA2%2FbtqE4Csupbr%2FNVJUoxUKuTMVMgqm8pDonk%2Fimg.png" width="800" height="300">

<br>

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FPuHTd%2FbtqE13ymgOR%2FOtNiJ9rF9ZWhwKAanVKyEK%2Fimg.png
" width="400" height="300">

### Full Outer Join(전체 외부 조인)

    왼쪽 테이블과 오른쪽 테이블의 합집합을 반환합니다. 양쪽 테이블에서 조인 조건에 맞는 행을 반환하며, 매칭되는 행이 없는 경우에는 NULL 값을 반환합니다.


### Inner Join VS Outer Join

     댓글이 있는 게시물만 보여줘라 VS 댓글이 없는 게시물도 보여줘라


<br>

# Nested Loop Join(중첩 조인)

    두 테이블을 조인하는 가장 기본적인 방법으로 하나의 테이블을 기준으로 기준 테이블의 행 마다 다른 테이블의 모든 행을 검색하는 방식. => 이중 for문과 유사

## 사용 케이스

- 작은 테이블 간의 조인이나, 인덱스가 적용된 테이블에 효과적

## 작동 방식

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FyPw0M%2FbtrbYtjXJCQ%2FurK6VRtm1T0c9cygsQIOfK%2Fimg.png" width="800" height="400">

- Table A가 Outer Table(선행 테이블), Driving Table(구동 테이블)

- Table B가 Inner Table(후행 테이블), Driven Table

1. 외부 테이블의 첫 번째 행을 가져옵니다.

2. 내부 테이블의 모든 행을 검색하면서, 조인 조건에 맞는 행을 찾습니다.

3. 조건에 맞는 모든 행을 찾았다면, 외부 테이블의 다음 행으로 이동하고, 내부 테이블의 검색을 다시 시작합니다.

4. 외부 테이블의 모든 행을 검사할 때까지 이 과정을 반복합니다.

### 그런데, 후행 테이블을 Index화 한다면 모든 테이블을 검색하지도 않아도 되어 속도를 빠르게 할 수 있다.

- index는 도서관의 도서 분류라고 보면 됨 => 모든 자료를 검색하지 않아도 된다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FAcmKI%2Fbtrb2bCzmEg%2FxL8X1rtMECBnM7dww7WRB1%2Fimg.png" width="800" height="400">

- Drivng Table의 범위가 작을수록 수행속도가 빨라진다. 따라서 Driving Table을 어떤 테이블로 설정하는지가 중요하다.

<br>

# Sort Merge Join

    각 테이블을 조인 키에 따라 정렬한 후, 두 테이블을 병합하면서 조인을 수행하는 방식

## 사용 케이스

- 두 테이블이 이미 정렬된 상태거나, 정렬된 인덱스가 존재할 때 효과적

- 출력해야 할 결과 값이 많을 때

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99F16B4E5BC6F1CA16" width="800" height="400">

## 작동 방식

1. 선행 테이블에서 조건을 만족하는 행을 찾고 조인 키를 기준으로 정렬

2. 후행 테이블에서도 같은 작업

3. Join 수행

## Hash Join

    해싱 기법을 이용하여 조인을 수행하는 기법이다.

<img src="https://mblogthumb-phinf.pstatic.net/MjAyMTAzMTJfNzMg/MDAxNjE1NTEzOTU2MjE5.84G3mzMzZVqYw41YO646oub5jZ8RhxAfI4j5Gn3Ox1kg.OdFsFijkhoRW99bOew1jsIBaECy9kp2Vr_o_Y723pFMg.PNG.dnjswls23/s1111sff.png?type=w800" width="800" height="400">

## 작동 방식

1. 선행 테이블에서 조건을 만족하는 행을 찾는다.

2. 선행 테이블의 조인 키로 해시함수를 적용하여 테이블을 생성한다. 이때, 조인 컬럼과 select 절에서 필요로 하는 컬럼도 함께 저장한다.

3. 1,2를 반복 수행한다.

4. 후행 테이블에서 조건을 만족하는 행을 찾는다.

5. 후행 테이블의 조인 키로 해시 함수를 적용하여 해당하는 버킷을 찾는다. => 조인 키를 이용하여 실제 조인된 데이터를 찾는다.

6. 조인 성공시 추출버퍼에 넣는다.

## 사용 케이스

- 조인 컬럼에 인덱스를 사용하지 않는다. 따라서, 조인 컬럼 인덱스가 없는 경우에도 사용가능하다.

- 해시 함수를 이용하여 조인을 하기 때문에 동등 연산 조인에서 사용한다. => 해시 함수가 적용될 때 동일한 값은 항상 같은 값으로 해싱 되기 때문.

## 특징

- 조인 시 해시 테이블을 메모리에 생성해야 한다.

- 생성된 해시 테이블의 크기가 메모리 적재 크기보다 커지면 임시 영역(디스크)에 저장한다. 이 과정은 추가적인 작업이 필요.

- 따라서, 결과 행의 수가 적은 것을 선행 테이블로 사용하는 것이 좋다.

- 해시 테이블을 만든 선행 테이블을 빌드 테이블이라고 하며, 후행 테이블을 프로브 테이블이라고 한다.
