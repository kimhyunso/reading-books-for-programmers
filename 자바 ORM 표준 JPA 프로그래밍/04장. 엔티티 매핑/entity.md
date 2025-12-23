# 엔티티 매핑 (★)

[`hibernate.hbm2ddl.auto`](http://hibernate.hbm2ddl.auto) : 테이블 CREATE되는 방법 (DDL)

| `create` | DROP + CREATE |  |
| --- | --- | --- |
| `create-drop` | DROP + CREATE + DROP |  |
| `update` | 엔티티와 테이블 정보를 비교하여 변경 사항만 수정한다. |  |
| `validate` | 엔티티와 테이블 정보를 비교하여 다르면 경고만 보낸다. |  |
| `none` | 자동생성 기능을 사용하지 않는다. |  |



## 4-1. 기본키 매핑

### 1. 기본 키 직접 할당 전략
- 모든 클래스 및 래퍼클래스, Primitive type(원시타입) 모두 사용 가능
    
    ```java
    @Id @Column(name = "member_id")
    private String id;

    entity.setId("member1");
    ```
    
### 2. IDENTITY 전략 [★★★]
- 기본 키 생성을 데이터베이스에 위임하는 전략
    
```sql
CREATE TABLE MEMBER(
        ID BIGINT NOT NULL AUTO_INCREMENT PRIMARY KEY
);
```
    
```java
@Id @Column(name = "member_id")
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```
    
### 3. SEQUENCE 전략
- 데이터베이스에서 생성할 수 있는 시퀀스 값을 기반으로 기본 키를 생성한다.
    
```sql
CREATE TABLE MEMBER(
        ID BIGINT NOT NULL PRIMARY KEY
);
-- 시퀀스 생성
CREATE SEQUENCE MEMBER_SEQ START WITH 1 INCREMENT BY 1;
-- CREATE SEQUENCE [sequenceName] START WITH [initialValue] INCREMENT BY [allocationSize];
```
    
```java
@Entity
@SequenceGenrator(
        name = "MEMBER_SEQ_GENERATOR",
        sequenceName = "MEMBER_SEQ",
        initialValue = 1, allocationSize = 1
)
public class Member{
        @Id @Column(name = "MEMBER_ID")
        @GeneratedValue(strategy = GenrationType.SEQUENCE,
                                        genrator = "MEMBER_SEQ_GENERATOR")
        private Long id;
}
```
    
### 4. TABLE 전략
- 기본 키 생성 전용 테이블을 하나 만들어서 이름과 값으로 데이터베이스 SEQUENCE를 흉내내는 전략
    
```sql
CREATE TABLE MY_SEQUENCES(
        sequence_name VARCHAR(255) NOT NULL,
        next_val bigint,
        primary key(sequence_name)
);
```
    
```java
@Entity
@TableGenerator(
        name = "MEMBER_SEQ_GENERATOR",
        table = "MY_SEQUENCES",
        pkColumnValue = "MEMBER_SEQ", allocationSize = 1
)
public class Member{
        @Id @Column(name = "MEMBER_ID")
        @GeneratedValue(strategy = GenrationType.TABLE,
                                        genrator = "MEMBER_SEQ_GENERATOR")
        private Long id;
}
```
    
### 5. AUTO 전략 (DEFAULT)
- 데이터베이스 방언에 따라 IDENTITY, SEQUENCE, TABLE 전략 중 하나를 자동을 선택한다.

## 4-2. 필드와 컬럼 매핑

| `@Column` | 컬럼을 매핑한다. |  |
| --- | --- | --- |
| `@Enumerated` | 자바의 enum 타입을 매핑한다. |  |
| `@Temporal` | 날짜 타입을 매핑한다. |  |
| `@Lob` | BLOB, CLOB 타입을 매핑한다. |  |
| `@Transient` | 특정 필드를 데이터베이스에 매핑하지 않는다. |  |
| `@Access` | JPA가 엔티티에 접그하는 방식을 지정한다. |  |
