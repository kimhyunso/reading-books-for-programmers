# 6. 다양한 연관관계 매핑

### 6.1 다대일

### 다대일 단방향[N:1]

```java
@Entity
public class Member{
		@Id @GeneratedValue
		@Column(name = "MEMBER_ID")
		private Long id;

		private String username;

		@ManyToOne
		@JoinColumn(name = "TEAM_ID")
		private	Team team;
}

@Entity
public class Team{
		@Id @GeneratedValue
		@Column(name = "TEAM_ID")
		private Long id;

		private String name;
}
```

### 다대일 양방향 [N:1, 1:N]

```java
@Entity
public class Member{
		@Id @GeneratedValue
		@Column(name = "MEMBER_ID")
		private Long id;
		private String username;

		@ManyToOne
		@JoinColumn(name = "TEAM_ID")
		private	Team team;

		// 편의 메소드
		public void setTeam(Team team){
				if (this.team != null){
						this.team.getMembers().remove(this);
				}
				this.team = team;
				team.getMembers().add(this);
		}
}

@Entity
public class Team{
		@Id @GeneratedValue
		@Column(name = "TEAM_ID")
		private Long id;
		private String name;

		@OneToMany(mappedBy = "team")
		private List<Member> members = new ArrayList<Member>();
}
```

### 6-2. 일대다

일대다는 `Collection`, `List`, `Set`, `Map`  중 하나를 사용해아함

### 일대다 단방향 관계 [1:N]

- 일대다 단방향 단점 : UPDATE SQL 추가로 실행한다.

```java
@Entity
public class Member{
		@Id @GeneratedValue
		@Column(name = "MEMBER_ID")
		private Long id;
		private String username;
}

@Entity
public class Team{
		@Id @GeneratedValue
		@Column(name = "TEAM_ID")
		private Long id;
		private String name;

		@OneToMany
		@JoinColumn(name = "MEMBER_ID")
		private List<Member> members = new ArrayList<Member>();
}
```

### 일대다 양방향 [1:N, N:1]

```java
@Entity
public class Member{
		@Id @GeneratedValue
		@Column(name = "MEMBER_ID")
		private Long id;
		private String username;

		@ManyToOne
		@JoinColumn(name = "TEAM_ID", insertable = false, updatable = false)
		private Team team;
}

@Entity
public class Team{
		@Id @GeneratedValue
		@Column(name = "TEAM_ID")
		private Long id;
		private String name;

		@OneToMany
		@JoinColumn(name = "MEMBER_ID")
		private List<Member> members = new ArrayList<Member>();
}
```

### 6-3. 일대일

### 일대일 단방향

```java
@Entity
public class Member{
		@Id @GeneratedValue
		@Column(name = "MEMBER_ID")
		private Long id;
		private String username;

		@OneToOne
		@JoinColum(name = "LOCKER_ID")
		private Locker locker;
}

@Entity
public class Locker{
		@Id @GeneratedValue
		@Column(name = "LOCKER_ID")
		private Long id;
		private String name;
}
```

### 일대일 양뱡향

```java
@Entity
public class Member{
		@Id @GeneratedValue
		@Column(name = "MEMBER_ID")
		private Long id;
		private String username;

		@OneToOne
		@JoinColum(name = "LOCKER_ID")
		private Locker locker;
}

@Entity
public class Locker{
		@Id @GeneratedValue
		@Column(name = "LOCKER_ID")
		private Long id;
		private String name;
	
		@OneToOne(mappedBy = "locker")
		private Member member;
}
```

### 6-4. 다대다[N:M]

중간에 연결 테이블을 추가해야한다.

### 다대다 단방향

```java
@Entity
public class Member{
		@Id @Column(name = "MEMBER_ID")
		private String id;
		private String username;

		@ManyToMany
		@JoinTable(name = "MEMBER_PRODUCT",
							joinColumns = @JoinColumn(name = "MEMBER_ID")
							inverseJoinColumns = @JoinColumn(name = "PRODUCT_ID"))
		private List<Product> products = new ArrayList<>();
}

@Entity
public class Product{
		@Id @Column(name = "PRODUCT_ID")
		private String id;
		private String name;
}
```

### 다대다 양방향

```java
@Entity
public class Member{
		@Id @Column(name = "MEMBER_ID")
		private String id;
		private String username;
	
		@ManyToMany
		@JoinTable(name = "MEMBER_PRODUCT",
							joinColumns = @JoinColumn(name = "MEMBER_ID")
							inverseJoinColumns = @JoinColumn(name = "PRODUCT_ID"))
		private List<Product> products = new ArrayList<>();
}

@Entity
public class Product{
		@Id @Column(name = "PRODUCT_ID")
		private String id;
		private String name;
	
		@ManyToMany(mappedBy = "products")
		private List<Member> members = new ArrayList<>();
}
```

### 다대다 : 연결 엔티티 사용 - 복합키

- 복합키 : [MEMBER_ID, PRODUCT_ID]
- `Serializable` 상속 시, 빈 생성자 생성, `equals()`, `hashCode()` :: `Override`

```java
@Entity
public class Member{
		@Id @Column(name = "MEMBER_ID")
		private String id;
		private String username;
		
		@OneToMany(mappedBy = "member")
		private List<MemberProduct> memberProducts = new ArrayList<>();
}

@Entity
public class Product{
		@Id @Column(name = "PRODUCT_ID")
		private String id;
		private String name;
	
		@OneToMany(mappedBy = "product")
		private List<MemberProduct> memberProducts = new ArrayList<>();
}

public class MemberProductId implements Serializable{
		private String member;
		private String product;
	
		public MemberProductId(){}
	
		@Override
	  public boolean equals(Object obj) {
	      return super.equals(obj);
	  }
	
	  @Override
	  public int hashCode() {
	      return super.hashCode();
	  }
}

@Entity
@IdClass(MemberProductId.class)
public class MemberProduct{
		@Id
		@ManyToOne
		@JoinColumn(name = "MEMBER_ID")
		private Member member;
	
		@Id
		@ManyToOne
		@JoinColumn(name = "PRODUCT_ID")
		private Product product;
	
		private int orderAmount;
}
```

### 다대다 : 연결 엔티티 사용 - 대리키

- 대리키 : [ORDER_ID]

```java
@Entity
public class Member{
		@Id @Column(name = "MEMBER_ID")
		private String id;
	
		private String username;
		
		@OneToMany(mappedBy = "member")
		private List<MemberProduct> memberProducts = new ArrayList<>();
}

@Entity
public class Product{
		@Id @Column(name = "PRODUCT_ID")
		private String id;
	
		private String name;
	
		@OneToMany(mappedBy = "product")
		private List<MemberProduct> memberProducts = new ArrayList<>();
}

@Entity
public class Order{
		// 대리키
		@Id @GeneratedValue
		@Column(name = "ORDER_ID")
		private Long id;
	
		@ManyToOne
		@JoinColumn(name = "MEMBER_ID")
		private Member member;
	
		@ManyToOne
		@JoinColumn(name = "PRODUCT_ID")
		private Product product;
	
		private int orderAmount;
}
```