
## 1. DDL : 데이터 정의어

> `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME`
> 
- 데이터 정의어로, 데이터 구조(테이블 스키마)를 정의(=객체를 생성, 삭제, 변경)하는 언어
    - DDL로 정의된 스키마는 데이터 사전에 저장된다.
- DROP은 객체 삭제, TRUNCATE는 데이터 삭제 기능이다. (= 스키마는 남김)
    - DROP 후엔 테이블 조회가 불가능하지만, TRUNCATE 후엔 테이블 조회는 가능하다.


> 📌 TRUNCATE는 객체가 아니라 데이터를 다루지만, **`AUTO COMMIT 속성`** 때문에 DDL에 속한다.



- AUTO COMMIT → 명령어 수행 후 고정되어 원복이 불가능함

---

1. **CREATE**
    - 객체(테이블, 인덱스 …)를 생성하는 명령어
    - 테이블 생성 시 각 컬럼의 제약조건 & 기본값 생략 가능
    - 테이블 복제에도 사용된다.
    
    ```sql
    CREATE TABLE [소유자] 테이블명 (
    	컬럼1 데이터타입 [DEFAULT 기본값] [제약조건],
    	컬럼2 데이터타입 [DEFAULT 기본값] [제약조건],
    	...);
    ```
    
2. **ALTER**
    - 테이블 구조를 변경할 수 있다. 컬럼 순서는 변경할 수 없다. (재생성 해야됨)
        - 컬럼 추가는 끝에만 가능하다 (원하는 위치 지정 X)
    - 컬럼 추가 : `ADD`
        - 컬럼 추가 시 제약 조건에 NOT NULL은 불가능하다 (**∵ 기본적으로 모두 NULL인 값을 갖고 컬럼이 추가됨)**
        - 여러 컬럼을 동시에 추가할 수 있다 (괄호 사용시)
    - 컬럼 수정 : `MODIFY`, `RENAME`
        - 컬럼 사이즈, 데이터타입, DEFAULT 값 변경 가능
        - 컬럼 사이즈는 제한없이 증가 가능하지만, 축소는 기존 데이터 중 최대 사이즈까지 가능
        - 데이터타입은 빈 컬럼일 경우만 가능하다 (CHAR, VARCHAR는 데이터 있어도 가능)
        - 이미 데이터 존재하는 컬럼에 DEFAULT 값 변경하면 기존 데이터는 수정되지 않고 그 이후부터 새로 적용된다
        - 컬럼 이름 변경(RENAME)은 항상 가능하지만, **동시에 여러 컬럼 이름 변경 불가**
    - 컬럼 삭제 : `DROP`
        - 데이터 존재 여부와 관계없이 상시가능
        - recyclebin에 남지 않기에 flashback으로 복구 할 수 없다
        - 괄호를 사용해 동시에 삭제할 수 없다
    
    ```sql
    # 테이블 이름 변경
    ALTER TABLE 기존테이블명 RENAME TO 새로운이름
    # 컬럼 추가
    ALTER TABLE 테이블명 ADD 컬럼명 데이터타입 [DEFAULT] [제약조건]
    # 여러 컬럼 추가 (괄호 사용)
    ALTER TABLE 테이블명 ADD **(**컬럼명1 데이터타입, 컬럼명2 데이터타입)
    ****# 속성 변경
    ****ALTER TABLE 테이블명 MODIFY (컬럼명 DEFAULT 값)
    # 컬럼명 변경
    ALTER TABLE TN RENAME COLUMN 기존컬럼명 TO 새컬럼명
    # 컬럼 삭제
    ALTER TABLE 테이블명 DROP 컬럼명
    ```
    
3. **DROP** 
    - 객체(DB, 테이블, 인덱스, 도메인, 뷰 등)를 삭제 시 사용
    - 옵션 : CASCADE (제거할 객체를 참조하는 모든 객체 제거), RESTRICT(다른 개체가 제거할 개체를 참조중이면 제거 취소)
    - 지워도 복구 가능
    - PURGE 옵션 → 영구삭제 (복구 불가능)
    
    ```sql
    DROP DATABASE 데이터베이스명
    DROP TABLE 테이블명 [PURGE];
    ```
    
4. **TRUNCATE**
    - 테이블 구조만 남기고 데이터만 즉시 삭제 후 즉시 반영 (AUTO COMMIT)
    - 로그를 남기지 않음
    
    ```sql
    TRUNCATE TABLE 테이블명;
    ```
    

## 2. DML : 데이터 조작어

> `SELECT`, `INSERT`, `DELETE`, `UPDATE`, `MERGE`
> 
- 데이터 베이스에 등록된 레코드를 *조회, 수정, 삭제*하는 작업을 통해 저장된 데이터를 실질적으로 처리하는데 사용
    - 데이터 검색 (⇒ DQL) : SELECT
    - 데이터 조작 : INSERT, UPDATE, DELETE, MERGE
- 비절차적 데이터 조작어 (사용자가 무슨 데이터를 원하는지만 명세)

- **저장(COMMIT) 또는 취소 (ROLLBACK)로 트랜잭션 제어가 필수적이다.**


1. **SELECT**
    - 데이터를 조회
    - SELECT 절에 표시할 대상 컬럼에 별칭을 지정할 수 있다 (AS - 생략 가능)
    - FROM쪽 테이블 별칭에는 AS 사용 불가능
        - 테이블 여러개 전달 시 조인 조건이 없다면 **카타시안 곱**이 발생함에 주의
    
    ```sql
    SELECT 불러올 연산 결과작성
    FROM 테이블
    [WHERE 조건]
    [JOIN 조인할 테이블명 ON 조인 기준 컬럼]
    [GROUP BY 그룹 기준 컬럼 HAVING 그룹 필터링 조건]
    [ORDER BY 정렬기준];
    ```
    
2. **INSERT**
    - 한 번에 한 행만 입력 가능하다 (SQL-Server 제외)
    - 컬럼명을 언급하지 않으면 NULL로 할당된다 (NOT NULL 조건과 충돌)
    - 문자 컬럼에 숫자 입력 가능, 숫자 컬럼에 ‘001’같은 문자형 가능
    
    ```sql
    INSERT INTO 테이블 VALUES(V1, V2, V3, ... );
     
    INSERT INTO 테이블(C1, C3) VALUES(V1, V3);
    
    # 오류 -> INTO 테이블 VALUES(V1, V3);
    ```
    
3. **UPDATE**
    - 컬럼 단위 수행, 다중 컬럼 수정 가능
    
    ```sql
    #단일 컬럼 수정
    UPDATE 테이블명 
    SET 수정할컬럼명 = 수정값
    WHERE 조건; -- WHERE절로 수정 대상 선택, 생략 하면 모든 행 수정
    
    #다중 컬럼 수정
    UPDATE 테이블명
    **SET 수정컬럼명1 = 수정값1, 수정컬럼명2 = 수정값2, ...** 
    WHERE 조건;
    ```
    
4. **DELETE**
    - 행 단위 실행
    - ***WHERE조건을 입력하지 않은 경우 전체 데이터 삭제*** (≠ DROP)
    
    ```sql
    DELETE [FROM] 테이블명
    [WHERE 조건];
    ```
    
5. **MERGE**
    - 테이블을 병합하는 작업을 수행한다.
    - 소스 테이블에서 타켓 테이블로 INSERT, UPDATE, DELETE 작업을 동시에 수행한다.
    
    ```sql
    MERGE INTO 테이블명 -- 수정할 테이블 명
    USING 참조테이블
       **ON (연결조건)** -- **괄호 생략 불가능!!**
     WHEN MATCHED THEN
    		  UPDATE SET 수정할 내용 
     WHEN NOT MATCHED THEN
    	    INSERT VALUES(삽입할내용); --INTO는 MERGE 뒤에 있으므로 생략
    ```
    

## 3. DCL : 데이터 제어언어

> `GRANT`, `REVOKE`
> 
- 객체에 대한 권한을 부여하거나 회수한다
    - 테이블 소유자는 타 계정에 테이블 조회 · 수정 권한 부여/회수가 가능하다.
- 권한 관리를 통해 시스템 보안을 유지하는 역할을 수행한다.

### 권한

- 일반적으로 본인(접속한 계정) 소유가 아닌 테이블은 원칙적으로 조회 불가(권한 통제)
- 업무적으로 필요시 테이블 소유자가 아닌 계정에 테이블 조회, 수정 권한 부여 필요

### 권한 종류

1. 오브젝트 권한
    
    - 테이블에 대한 권한 제어 (특정 테이블에 대한 SELECT, INSERT, UPDATE, DELETE, MERGE 권한)
    
    - 해당 **테이블 소유자**는 타 계정에게 **소유 테이블에 대한** 조회 및 수정 권한 부여/회수 가능
    
2. 시스템 권한
    
    - 시스템 작업(테이블 생성 등) 제어 (EX. 테이블 생성 권한, 인덱스 삭제 권한)
    
    - **관리자 권한 가지고 있는 사람만** 권한 부여/회수 가능
    


> 💡 DCL 명령어 입력하는 순간 해당 작업이 즉시 AUTO COMMIT 된다! <br> (**∵** 데이터 베이스의 테이블에 직접적으로 영향을 미치는 작업이기에)

1. **GRANT**
    - 권한 부여 시 반드시 테이블 소유자 OR 관리자 계정(SYS, SYSTEM)으로 접속해야 함
    - 동시에 가능⭕한 것
        - **여러 유저에 대한 권한** 부여
        - **(한 객체에 대한) 여러 권한** 부여
    - 동시에 불가능❌ 한 것
        - **여러 객체 권한** 부여
2. **REVOKE**
    - GRANT명령으로 주어진 액세스 권한 회수
        - 권한을 중복 회수한다면 에러 발생함
    - 동시에 2개 이상의 권한을 회수할 수 있고, 2명 이상의 유저의 권한을 회수할 수 있다.
    
    ```sql
    # 권한 부여
    GRANT SELECT, UPDATE, INSERT ON TB TO HR;
    # 권한 회수
    REVOKE SELECT, UPDATE, INSERT ON TB FROM HR;
    ```
    

## 4. TCL : 트랜잭션 제어언어

> `COMMIT`, `ROLLBACK`, `SAVEPOINT`
> 
- 데이터를 제어하는 언어로, DML에 의해 조작된 결과를 작업단위(트랜잭션) 별로 제어하는 명령어
- 데이터의 보안, 무결성, 회복, 병행 수행제어 등을 정의하는데 사용한다
- DML 수행 후 트랜잭션을 정상 종료하지 않은 경우 ⇒ **LOCK이 발생할 수 있다**

### LOCK

- 트랜잭션이 수행하는 동안 **특정 데이터에 대해 다른 트랜잭션의 동시 접근 제한**
- 잠금이 걸린 데이터 = 잠금을 실행한 트랜잭션만이  접근&해제 가능 (관리자 권한 계정 제외)

### 트랜잭션

- 데이터베이스의 **분할 할 수 없는** 논리적 연산 **최소 단위**(하나의 연속적인 업무 단위)
- 1 트랜잭션 → 1개 이상의 SQL문장 포함함
- **ALL 혹은 NOTHING 개념 (모두 COMMIT 하거나 ROLLBACK 처리)**

```sql
# 트랜잭션 시작
BEGIN TRANSACTION -- 또는 BEGIN TRAN
# 트랜잭션 종료
COMMIT (TRANSACTION)
# 취소
ROLLBACK
```

### 트랜잭션 특징

- 원자성 : 트랜잭션 정의된 연산들 모두 성공적으로 실행 OR 전혀 실행되지 않은 상태
- 일관성 : 실행 전 DB에 이상 없으면 실행 후에도 이상 없어야 함
- 고립성 : 실행 중 다른 트랜잭션 영향 받으면 안됨
- 지속성 : 성공적으로 수행되면 갱신된 내용 영구 저장

---

1. **COMMIT**
    - 이상 없는 경우 작업 완료된 데이터를 데이터베이스에 영구적으로 반영
    - 한 번 COMMIT 수행하면 → COMMIT 이전에 수행된 DML은 모두 저장된다
        
        IF) DML1 - DML2 - DDL 수행하면 AUTO COMMIT으로 그전 DML1, DML2 까지 자동 커밋됨
        
    - COMMIT 없이(혹은 LOCK이 걸린 상태로) 종료하면 자동 ROLLBACK 처리

1. **ROLLBACK**
    - 테이블 내 입력/수정/삭제한 데이터에 대해 변경을 취소
    - DB내 저장되지 않고 최종 COMMIT 지점/변경 전/특정 SAVEPOINT 지점으로 원복됨
        - ***SAVEPOINT 지점 이후, COMMIT이 발생했다면 돌아갈 수 없음***
    - ROLLBACK 가능한 범위 = 최종 COMMIT 시점 이전
    - SAVEPOINT 설정 → 최종 COMMIT 시점 아닌 **그 이후** 원하는 시점으로 가능 (이전은 불가능!)

1. **SAVEPOINT**
    - 트랜잭션 내에서 롤백을 부분적으로 수행하기 위해 사용되는 지점을 지정하는데 사용한다
    - 사용자가 원하는 위치에 원하는 이름으로 설정 가능하다
    - `ROLLBACK TO savepoint_name` 으로 원하는 지점으로 원복 가능하다 **(**❗**COMMIT 이전으로는 불가능)**
    
    ```sql
    # 오라클
    SAVEPOINT SVPT1; --라벨 붙임
    ...
    ROLLBACK TO SVPT1;
    
    # SQL-Server
    SAVE TRANSACTION SVPT1;
    ...
    ROLLBACK TRANSACTION SVPT1;
    ```
    

## **5. JOIN**

- **여러 테이블의 데이터를 사용하여 동시 출력하거나 참조하는 경우**
- **동일한 열 이름이 여러 테이블에 존재할 경우 열 이름 앞에 출처 밝혀줌(별칭)**
- N개의 테이블 조인하려면 **최소 N-1개의 조인 조건** 필요
- MySQL은 JOIN, MongoDB에서는 lookup 사용
    - NoSQL은 RDBMS 보다 JOIN 연산 성능이 떨어진다 ⇒ 조인 작업 많은 경우 RDBMS 권장

### JOIN 종류

- **INNER JOIN**
    - 조인 조건이 일치하는 행만 추출해 표기함
    - NULL이 결과로 나올 수 없다.
    - `USING` 이나 `ON` 조건 절 명시 필수
        
        USING : 조인용 컬럼 명이 같은 경우 사용
        
        ON : 양 컬럼 이름이 다른 경우
        
- **NATURAL JOIN**
    
    두 테이블간 같은 모든 컬럼에 대해 EQUI JOIN (값이 같으면 조합)
    
- **CROSS JOIN**
    
    JOIN 조건 없는 경우 모든 테이블들의 조합 (= 카타시안 곱)
    
- **OUTER JOIN**
    
    일치하지 않는 데이터도 포함해 테이블에서 반환
    
    - **LEFT OUTER JOIN**
        - 왼쪽 테이블을 기준으로 합쳐서 출력
        - 오른쪽 테이블에서 빈 값은 NULL로 출력 (왼쪽 테이블 완전 출력)
    - **RIGHT OUTER JOIN**
        - 오른쪽 테이블 기준 → 왼쪽 데이터 채움 (오른쪽 테이블 완전 출력)
        - FROM절에 나오는 순서에 따라 LEFT, RIGHT 순서 결정
    - **FULL OUTER JOIN (합집합 조인)**
        - 두 테이블 전체 기준으로 결과 생성 → 중복 데이터는 삭제 후 리턴
        - LEFT OUTER JOIN `UNION` RIGHT OUTER JOIN 한 결과
    
    ### JOIN 원리
    
    조인의 원리에는 중첩 루프 조인, 정렬 병합 조인, 해시 조인이 있다.
    
    1. **Nested Loop Join : 중첩 루프 조인**
        - 중첩 for문과 같은 원리로 조건에 맞는 조인을 하는 방법
        - 랜덤 접근에 대한 비용이 ⬆️  **∴** 대용량 테이블에서는 사용 안함
    2. **Sort Merge Join : 정렬 병합 조인**
        - 각각의 테이블을 조인할 필드 기준으로 정렬한 후, 조인 작업 수행하는 방식
        - 조인에 사용할 적절할 인덱스가 없는 경우, 대용량의 테이블들을 조인하고 조인 조건으로 <, >와 같은 범위 비교 연산자가 있을 때 사용함
    3. **Hash Join : 해시 조인**
        - 해시 테이블을 기반으로 조인하는 방법이다
        - 테이블 조인 시 한 테이블이 메모리에 온전히 들어간다면 중첩 루프 조인보다 효율적이다.
        - 동등 조인(EQUI JOIN)에서만 사용할 수 있음
    - **해시 조인 단계 : 빌드 단계, 프로브 단계**
        - 빌드단계 : 크기 작은 테이블로 인메모리 해시 테이블 빌드 (→ 조인 기준 키 = 해시 테이블 키)
        - 프로브 단계 : 레코드 읽으며 각 레코드에서 조인 키에 일치하는 레코드들 찾아 반환
            - 각 테이블은 1번씩 읽음 (중첩 루프 조인보다 보통 성능 굿)
        
        1. 빌드 단계
            
            입력 테이블 중 (바이트 더 작은) 하나를 기반으로 인메모리 해시 테이블을 빌드한다
            
            조인에 사용되는 필드가 해시 테이블의 키로 사용된다
            
        2. 프로브 단계
            
            이 단계 동안 레코드를 읽으며, 각 레코드에서 조인 기준 키에 일치하는 레코드를 찾아 결괏값으로 반환한다
            
            → 각 테이블은 한 번씩만 읽게 되기에 중첩해서 2개의 테이블을 읽는 중첩 루프 조인보다 성능이 좋다. (사용 가능한 메모리양은 시스템 변수 join_buffer_size에 의해 제어되며, 런타임 시 조정 가능하다)
            

## 6. 실행 순서

- SELECT문의 내부 파싱(문법적 해석) 순서 ≠ 나열된 순서


>🚨 **실행 순서 <br>
FROM → {ON → JOIN →} WHERE → GROUP BY → HAVING → SELECT → {DISTINCT →} ORDER BY**


- WHERE 절에 있는 내용을 HAVING절에서 사용할수 있지만 실행 순서에 의해 성능이 떨어진다
- SELECT에서 Alias를 설정했을때, **ORDER BY에서만 별칭을 사용**할 수 있다

---

https://technote-mezza.tistory.com/93

https://jjuya.tistory.com/58

https://hstory0208.tistory.com/entry/데이터베이스-DDL-DML-DCL이란

https://ssd0908.tistory.com/entry/MySQL-DDL-DML-DCL-명령어

https://monawa.tistory.com/100

https://bitcodic.tistory.com/m/178

https://velog.io/@dbfla0628/면접을-위한-CS-전공지식-노트-Chapter-04.-데이터베이스-조인의-종류-조인의-원리