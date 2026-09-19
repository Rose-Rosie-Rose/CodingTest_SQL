## SQL_syntax
### JOIN 종류 구분하기

JOIN 문제에서는 먼저 **"매칭되는 데이터가 없어도 해당 행을 출력해야 하는가?"**를 생각한다.

> **연결되는 데이터가 없어도 출력해야 하는가?**
> - NO → `INNER JOIN`
> - YES → `LEFT JOIN`

#### INNER JOIN

두 테이블에서 **서로 연결되는 데이터만 조회**할 때 사용한다.

```sql
SELECT *
FROM A
JOIN B
    ON A.id = B.id;
```

- `JOIN`만 작성하면 기본적으로 `INNER JOIN`
- 양쪽 테이블에 연결되는 데이터가 있는 경우만 조회
- 연결되는 데이터가 없으면 결과에서 제외

##### 문제에서 자주 나오는 표현

- A와 B에서 조건에 맞는 데이터 조회
- A에 존재하는 B 조회
- 게시글에 작성된 댓글 조회
- 주문한 고객 조회

##### 예시

**댓글이 작성된 게시글만 조회**

```sql
SELECT *
FROM BOARD B
JOIN REPLY R
    ON B.BOARD_ID = R.BOARD_ID;
```

결과:

```text
BOARD     REPLY
------------------
B001      댓글1
B002      댓글2
```

댓글이 없는 게시글은 결과에서 제외된다.

#### LEFT JOIN

**왼쪽 테이블의 데이터는 매칭되는 데이터가 없어도 모두 조회**할 때 사용한다.

```sql
SELECT *
FROM A
LEFT JOIN B
    ON A.id = B.id;
```

`FROM` 뒤에 작성한 **왼쪽 테이블 A의 데이터는 모두 유지**된다.

B에 연결되는 데이터가 없다면 B의 컬럼은 `NULL`로 출력된다.

```text
A         B
------------------
1         데이터
2         데이터
3         NULL
```

##### 문제에서 자주 나오는 표현

- 모든 A를 조회
- B가 없는 경우도 포함
- 주문하지 않은 고객도 포함
- 댓글이 없는 게시글도 포함
- 데이터 존재 여부와 관계없이 전체 조회

##### 예시

**댓글이 없어도 모든 게시글 조회**

```sql
SELECT *
FROM BOARD B
LEFT JOIN REPLY R
    ON B.BOARD_ID = R.BOARD_ID;
```

결과:

```text
BOARD     REPLY
------------------
B001      댓글1
B002      댓글2
B003      NULL
```

댓글이 없는 `B003`도 결과에 포함된다.


#### INNER JOIN vs LEFT JOIN

| 구분 | INNER JOIN | LEFT JOIN |
| --- | --- | --- |
| 매칭된 데이터 | 조회 | 조회 |
| 매칭되지 않은 왼쪽 데이터 | 제외 | 조회 |
| 오른쪽 데이터가 없는 경우 | 행 자체가 제외 | `NULL` |
| `JOIN`만 작성 | 가능 | `LEFT JOIN` 명시 |
| 주로 사용하는 경우 | 서로 연결된 데이터만 필요 | 왼쪽 데이터를 모두 유지해야 함 |

---

#### 문제에서 JOIN 선택하는 방법

##### 1. 두 테이블을 연결해야 하는가?

두 테이블의 컬럼이 필요한 경우 JOIN을 생각한다.

```text
게시글 테이블 → 게시글 제목
댓글 테이블   → 댓글 내용
```

두 테이블의 데이터가 모두 필요하므로 JOIN이 필요하다.

##### 2. 매칭되지 않는 데이터도 필요한가?

**필요하지 않음**

→ `INNER JOIN`

```sql
FROM BOARD B
JOIN REPLY R
    ON B.BOARD_ID = R.BOARD_ID
```

**필요함**

→ `LEFT JOIN`

```sql
FROM BOARD B
LEFT JOIN REPLY R
    ON B.BOARD_ID = R.BOARD_ID
```

#### 핵심 판단법

```text
연결되는 데이터가 없어도 출력해야 하나?
              │
        ┌─────┴─────┐
        │           │
       NO          YES
        │           │
   INNER JOIN    LEFT JOIN
```

기본적으로 **`INNER JOIN`을 먼저 생각**하고,

문제에서 다음과 같은 표현이 나오면 `LEFT JOIN`을 확인한다.

- **모든**
- **전체**
- **~가 없는 경우도 포함**
- **~하지 않은 데이터도 조회**
- **존재 여부와 관계없이**

> **한 줄 암기**
>
> 연결 안 돼도 살려야 한다 → `LEFT JOIN`  
> 연결된 것만 필요하다 → `INNER JOIN`
