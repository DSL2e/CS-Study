# 1. NoSQL
NoSQL은 흔히 **"Not only SQL"**로 풀어서 설명한다. 이는 NoSQL이 SQL을 사용하지 않는 것이 아니라 SQL뿐만 아니라 다양한 방식의 데이터 엑세스를 제공함을 의미한다.

NoSQL은 전통적인 관계형 데이터베이스와 달리, 정형화된 테이블 구조가 아닌 다양한 형태의 데이터 모델을 사용한다.

NoSQL은 정형화된 스키마가 없고, 수평적 확장이 용이하여 대규모 데이터 처리와 비정형 데이터를 다루는 웹 애플리케이션, 실시간 데이터 처리 등에 적합하다.

## 1-1. NoSQL 유형
### Key-Value Model
**키와 값**을 쌍으로 저장하는 단순한 구조를 가진다.

값에 들어가는 데이터 형식에 제약이 없으며, 주로 성능 향상을 위한 데이터 캐싱이나 사용자 세션 정보 저장 등에 사용된다.

예) redis, memcahced, AWS DynamoDB

### Document Model
JSON이나 XML 등의 형식을 가진 **문서**를 저장하는 모델이다.

유연한 스키마를 제공한다.
- 값을 추가하기 전에 스키마를 별도로 정의하지 않으며, 문서가 추가되면 자동으로 스키마가 된다.
- 동일한 컬렉션 내에 문서가 서로 다른 필드를 가질 수 있다.

주로 다양한 속성이 있는 데이터를 사용하거나 JSON과 같은 문서 구조 혹은 비정규화된 중첩 구조를 사용하는 애플리케이션에서 사용된다.

예) MongoDB

### Wide-Column Model
> 
와이드 컬럼 스토어(wide column store)는 NoSQL 데이터베이스의 유형이다. 테이블, 로우, 컬럼을 사용하지만 관계형 데이터베이스와는 달리 컬럼의 이름과 포맷은 동일한 테이블의 로우마다 다를 수 있다. 와이드 컬럼 스토어는 2차원 키-값 스토어로 해석할 수 있다.
>
-위키백과 -

행과 열 기반으로 데이터를 저장하지만 각 행마다 가지는 열(column)의 종류 및 수는 다를 수 있다.

한 객체에 관한 정보를 매우 넓은 단일 행에 저장한다.
하나의 행에 방대한 수의 컬럼 정보가 들어갈 수 있다. (wide-column)

여러 컬럼들을 하나로 묶어 표현할 수도 있다. 이렇게 묶인 컬럼들을 컬럼 패밀리(Column Family)라고 부른다.

![](https://velog.velcdn.com/images/jjbin/post/ea7f91f5-f897-433a-8023-b5baf501418a/image.png)

예) Hbase, Cassandra

### Graph Model
노드와 엣지로 구성된 데이터 모델로, 복잡한 관계형 데이터를 처리하는 데 사용된다.

데이터를 노드로 표현하며 노드 간 관계를 엣지로 연결하여 그래프를 구성한다.

주로 소셜 네트워크, 예약 시스템, 사기 감지 등에 사용된다.

예) Neo4j

---

## 1-2. NoSQL 장단점
### 장점
**수평적 확장에 유리**: 분산 시스템을 통해 상대적으로 손쉽게 확장 가능하다.
**유연한 데이터 구조**: 비정형 및 반정형 데이터를 처리할 수 있다.
**높은 성능**: 대용량 데이터를 빠르게 처리할 수 있다.

### 단점
**복잡한 쿼리 처리에 제한**: 관계형 데이터 처리에는 부적합하다.
**데이터 일관성 문제**: 여러 노드 간 데이터 일관성을 보장하기 어렵다.



### SQL과 NoSQL 비교
|SQL|NoSQL|
|:---|:---|
|Stands for Structured Query의 약자|Not Only SQL의 약자|
|미리 정의된 스키마가 있는 구조화된 데이터에 적합|비정형 및 반정형 데이터에 적합|
|트랜잭션 관리를 위해 ACID 속성을 따름| 반드시 ACID 속성을 따르지는 않음|
|JOIN 및 복잡한 쿼리 지원 O|JOIN 및 복잡한 쿼리 지원 X|
|수직 확장 (scale up)|수평 확장(scale out)|
|예: MySQL, PostgreSQL, Oracle, MSSQL|예: MongoDB, Cassandra, Couchbase, Amazon DynamoDB, Redis|

---

# 2. 분산 시스템
NoSQL은 수평적 확장에 용이하여 분산 시스템을 설계하는 데 적합하다.

분산 시스템을 설계하는 데에 있어 고려해야 하는 점들에 대해 알아보자.

## 2-1. 분산 시스템의 세 가지 특성
### Consistency
일관성이 보장되는 분산 시스템에 접속하는 모든 클라이언트는 어떤 노드에 접속했느냐에 관계없이 동시에 같은 데이터를 조회할 수 있다.

이를 위해서는 데이터가 한 노드에 기록될 때마다 다른 모든 노드에 데이터를 즉시 전달 및 복제해야 한다.

### Availability
가용성이 보장되는 분산 시스템의 일부 노드에 장애가 발생하더라도 모든 클라이언트는 정상적으로 응답받을 수 있다.

### Partition Tolerance
분산 시스템에 네트워크에 파티션(분할)이 생기더라도 시스템(클러스터)은 계속 동작해야 한다.

의미상 분할 내성보다는 분할 용인 혹은 허용이라고 번역하는 게 적절하다.

- 파티션은 분산 시스템 내 통신 장애가 발생하여 노드 간의 연결이 끊어지거나 일시적으로 지연되는 것을 의미한다.

## 2-2. CAP 정리
> 
CAP 정리는 데이터 **일관성(consistency)**, **가용성(availability)**, **파티션 감내(partition tolerance)**라는 세 가지 요구사항을 동시에 만족하는 분산 시스템을 설계하는 것은 불가능하다는 정리다.
>
-가상 면접 사례로 배우는 대규모 시스템 설계 기초-


CAP 정리에 의해 NoSQL 데이터베이스는 세 가지 중 두 가지 특성만을 충족할 수 있다.

따라서 필요에 따라 적합한 데이터베이스를 선택해야 한다.


### CP 시스템
Consistency와 Partition tolerance를 지원한다.

네트워크 분할이 발생하면 분할이 해결될 때까지 시스템을 사용할 수 없다.

MongoDB는 CP 시스템으로, 마스터 노드를 사용할 수 없게 되면 새 마스터 노드가 선정되고 클러스터의 일관성이 다시 보장되기 전까지 클러스터를 사용할 수 없다.

### AP 시스템
Availability와 Partition tolerance를 지원한다.

네트워크 분할이 발생해도 시스템으로부터 정상적으로 응답받을 수 있지만 그 결과가 일관되지 않을 수 있다.

Cassandra는 AP 시스템으로, 마스터 노드가 없어 클러스터 내 아무 노드에 쓰기 작업을 수행할 수 있다.
따라서 네트워크 분할이 발생해도 정상적으로 노드에 작업을 진행할 수 있지만 각 노드가 가지고 있는 데이터가 일치하지 않을 수 있다. 

이러한 문제는 이후 결과적 일관성을 통해 해결한다.


### CA 시스템
Consistency와 Availability를 지원한다.

현실 세계에 CA 시스템인 분산 시스템은 존재할 수 없다.
네트워크 장애는 피할 수 없는 일이기 때문이다.

따라서 **분산 시스템은 반드시 Partition tolerance를 지원해야 한다.**

### P를 포기할 수는 없다
CAP 정리를 보면 마치 P를 포기할 수 있는 것처럼 보이지만 P는 포기할 수 없다.

C, A와 달리 **P는 분산 시스템이 아닌 분산 시스템이 돌아가는 네트워크에 대한 특성**이기 때문이다.
현실에서 완벽히 장애가 없는 네트워크를 만들 수는 없다.

따라서 분산 시스템은 반드시 Partition tolerance를 지원해야 한다.

### CAP 정리는 네트워크 장애 상황에 관한 것이다
CAP 정리가 내포하고 있는 의미는 분산 시스템은 **네트워크 장애 상황에서 일관성과 가용성 중 하나만 선택할 수 있다**는 것이다.

억지로 예를 들어 보자면 네트워크 장애가 없는 상황에서는 AP 시스템도 얼마든지 일관성(C)을 유지할 수 있다.
(하지만 P를 선택했는데 네트워크 장애가 없는 상황이라는 것이 모순이다...)

## 2-3. PACELC
CAP는 네트워크 장애 상황에 대한 선택에 대해 도움을 주지만 정상 상황에서는 도움을 주지 못한다.

PACELC는 CAP를 보완하기 위한 표기법으로, **장애 상황과 정상 상황일 때를 나누어 설명**한다.

>if there is a partition (P) how does the system tradeoff between availability and consistency (A and C); else (E) when the system is running as normal in the absence of partitions, how does the system tradeoff between latency (L) and consistency (C)?

![](https://velog.velcdn.com/images/jjbin/post/22bf11ee-651d-4eb6-b466-9c24b825a991/image.png)

**장애 상황(P)**에서는 **가용성(A)**과 **일관성(C)**을 트레이드 오프
**정상 상황(E)**에서는 **Latency(L)**와 **일관성(C)**을 트레이드 오프

- L은 클러스터 내 전달되는 메시지의 Latency를 의미

<br>
예를 들어 PA/EL 시스템은 장애 상황에서 일관성을 가용한 노드에만 데이터를 쓴다.<br>
정상 상황에서도 Latency를 줄이기 위해 일관성을 포기하고 일부 노드에만 데이터를 쓴다.

- 데이터들의 일관성은 차후에 결과적 일관성(Eventually Consistency)을 통해 달성한다.



---

## 2-4. BASE 원칙
BASE는 ACID와 같이 **트랜잭션 처리 중에 데이터베이스가 작동하는 방식을 나타내는 데이터베이스 속성**을 의미한다.

**BASE** 모델은 일관성을 희생하고 성능과 가용성을 중시한다.
- **B**asically **A**vailable
- **S**oft state
- **E**ventually consistent

### Basically Available
시스템은 기본적으로 항상 이용 가능해야 한다.

데이터의 일관성을 포기하는 대신, 가용성을 제공한다.

### Soft State
쓰기 작업 등 별도의 트리거 없이도 **데이터의 상태 정보가 시간에 따라 변할 수 있다**.

시간이 지남에 따라 결과적 일관성을 보장하기 위해 상태가 변경될 수 있음을 의미한다.

### Eventually Consistency
일정 기간 이후에는 데이터 일관성을 유지한다.

여러 사용자가 동시에 입력을 진행하면 가용성을 위해 일관성이 유지되지 않을 수 있다.
하지만 시간이 지남에 따라 시스템은 각 변경 내용을 전파하고 병합하여 **최종적으로는 모든 노드가 같은 상태를 같도록 한다**.

이를 결과적으로 일관성을 보장한다고 한다.

결과적으로 일관성을 유지하는 가장 유명한 시스템으로는 DNS가 있다.

>인터넷 DNS(도메인 이름 시스템)는 eventual consistency 모델이 사용된 시스템의 예로 잘 알려져 있습니다. DNS 서버가 항상 최신의 값을 반영하는 것은 아니며, 이러한 값들은 인터넷상의 수많은 디렉터리에서 캐싱되고 복제됩니다. 수정된 값을 모든 DNS 클라이언트와 서버에 복제하려면 어느 정도의 시간이 소요됩니다. 하지만 DNS 시스템은 인터넷의 근간을 이루는 요소로 자리잡은 매우 성공적인 시스템입니다. DNS는 가용성이 매우 높으며 엄청난 확장성이 증명되었고, 인터넷 전체에서 수천만 대 기기의 이름 조회를 가능하게 하고 있습니다.
>
-Google Cloud-

<br>

![](https://velog.velcdn.com/images/jjbin/post/9bae1d22-808e-4952-b37a-141e0c5fdce9/image.png)

위 그림은 BASE를 따르는 시스템이 동작하는 방식을 잘 보여준다.

누군가가 X를 쓰더라도 일부 가용한 노드만이 갱신되며 클러스터 내 데이터 일관성이 깨진다.
하지만 사용자는 오래된 데이터를 읽을지언정 항상 시스템에 접근 가능하다. **(Basically Available)**
또한 시간이 지남에 따라 오래된 데이터를 가진 노드 또한 업데이트 된다. **(Soft State)**
결과적으로 데이터는 일관된 상태를 갖게된다. **(Eventually Consistency)**

### ACID와 BASE 비교

![](https://velog.velcdn.com/images/jjbin/post/84a7226b-c062-4f9b-9ba8-7e4fc2210c99/image.png)


# References
[NoSQL이란 무엇인가?](https://www.oracle.com/kr/database/nosql/what-is-nosql/)

[NoSQL이란 무엇인가? 대량데이터 동시처리위한 DBMS 종류와 특징](https://www.samsungsds.com/kr/insights/1232564_4627.html)

[SQL과 NoSQL 비교:
5가지 주요 차이점](https://www.integrate.io/ko/blog/the-sql-vs-nosql-difference-ko/)

[NoSQL 데이터베이스별 특징](https://jaemunbro.medium.com/nosql-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%ED%8A%B9%EC%84%B1-%EB%B9%84%EA%B5%90-c9abe1b2838c)

[NoSQL DB 종류](https://bestpractice80.tistory.com/66)

[그래프 데이터베이스 정의](https://www.oracle.com/kr/autonomous-database/what-is-graph-database/)

[분산 시스템에서의 데이터 일관성과 CAP 이론](https://f-lab.kr/insight/data-consistency-and-cap-theorem-in-distributed-systems)

[CAP 정리란?](https://www.ibm.com/kr-ko/topics/cap-theorem)

[CAP Theorem, 오해와 진실](http://eincs.com/2013/07/misleading-and-truth-of-cap-theorem/)

[ACID 데이터베이스와 BASE 데이터베이스의 차이점은 무엇인가요?](https://aws.amazon.com/ko/compare/the-difference-between-acid-and-base-database/)

[Datastore로 strong consistency와 eventual consistency 간에 균형 유지](https://cloud.google.com/datastore/docs/articles/balancing-strong-and-eventual-consistency-with-google-cloud-datastore?hl=ko)
