---
title: "[JPA]연관 관계 매핑"
categories: [API, JPA]
tags: [API, JPA, 연관 관계, 단방향 객체 연관 관계, 양방향 객체 연관 관계, mapping, 매핑]
image:
  path: /assets/img/posts/api/jpa/01-jakarta-ee-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Jakarta EE
---

# 연관 관계 매핑

## 연관 관계의 중요성

![01-no-relationship-design](/assets/img/posts/api/jpa/relationship-mapping/01-no-relationship-design.jpg)
*객체 연관 관계가 없는 설계 방식*

위 사진은 **연관 관계를 고려하지 않는 설계 방식**을 나타낸 것이다. 즉, `Member` 객체는 `Team` 객체의 외래 키(`teamId`)를 가지고 있을 뿐 `Team` 객체를 참조하고 있지 않다. 

이러한 설계 방식은 객체를 테이블에 맞추어 **데이터 중심으로 모델링**한 것으로, 주로 관계형 DB 모델링의 관점에서 접근했기 때문에 발생한다.

```java
@Entity
public class Member {

    @Id @GeneratedValue
    private Long id;

    @Column(name = "USERNAME")
    private String name;

    private int age;

    @Column(name = "TEAM_ID")
    private Long teamId;

    // ...
}

@Entity
public class Team {

    @Id @GeneratedValue
    private Long id;

    private String name;

    // ...
}
```

[사진](#연관-관계의-중요성)과 같이 참조 대신에 외래 키를 그대로 사용한다고 가정해보자.

<a id="anchor1"></a>

```java
// 팀 저장
Team team = new Team();
team.setName("TeamA");
em.persist(team);

// 회원 저장
Member member = new Member();
member.setName("member1");
member.setTeamId(team.getId());
em.persist(member);

// 조회
Member findMember = em.find(Member.class, member.getId());

// 연관 관계가 없음
Team findTeam = em.find(Team.class, team.getId());
```

DB의 테이블에서는 외래 키를 `JOIN`해서 연관된 데이터를 찾을 수 있지만, 객체는 참조를 통해 연관된 객체를 찾는다. 위 코드에서도 알 수 있듯이 객체 관계에서는 두 객체 사이에 연관 관계가 없다면 각 객체를 조회하기 위해 **각각의 객체를 조회**해야 하는 작업이 수반된다는 것을 알 수 있다. 테이블과 객체 사이에는 이런 큰 간극이 있다.

**객체를 테이블에 맞추어 데이터 중심으로 모델링하게 되면 협력 관계를 만들 수 없고, 이러한 설계 방식은 결코 객체 지향적이지 않다는 것이다.**

## 연관 관계 매핑 이론

### 단방향 매핑

![02-one-sided-relationship-design](/assets/img/posts/api/jpa/relationship-mapping/02-one-sided-relationship-design.jpg)
*단방향 객체 연관 관계가 있는 설계 방식*

반면에 위 사진은 **연관 관계를 고려한, 객체 지향적인 설계 방식**이다. `Member` 객체는 외래 키(`teamId`)를 참조하는 것이 아닌, `Team` **객체 자체를 직접적으로 참조**하고 있다. **객체 연관 관계만 바뀌었을 뿐 테이블 연관 관계는 바뀌지 않았다.**

이때 `Member` 객체는 `Team` 객체를 참조할 수 있지만, `Team` 객체는 `Member` 객체를 참조할 수 없는 구조이므로 단방향 관계다.

![03-one-sided-relationship-mapping](/assets/img/posts/api/jpa/relationship-mapping/03-one-sided-relationship-mapping.jpg)
*단방향 객체 연관 관계 매핑*

```java
@Entity
public class Member {

    @Id @GeneratedValue
    private Long id;

    @Column(name = "USERNAME")
    private String name;

    private int age;

//  @Column(name = "TEAM_ID")
//  private Long teamId;

    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;

    // ...
}

@Entity
public class Team {

    @Id @GeneratedValue
    private Long id;

    private String name;

    // ...
}
```

`Team` 객체 입장에서는 하나의 `Team` 객체가 여러 `Member` 객체와 관계하기 때문에 일대다(1:N) 관계이지만, `Member` 객체 입장에서는 여러 `Member` 객체가 하나의 `Team` 객체와 관계하는 다대일(N:1) 관계이므로 `@ManyToOne` 어노테이션을 사용해서 두 객체 사이의 연관 관계를 표현할 수 있다.

```java
// 팀 저장
Team team = new Team();
team.setName("TeamA");
em.persist(team);

// 회원 저장
Member member = new Member();
member.setName("member1");
member.setTeam(team); // 단방향 연관 관계 설정, 참조 저장
em.persist(member);

// 조회
Member findMember = em.find(Member.class, member.getId());

// 참조를 사용해서 연관 관계 조회
Team findTeam = findMember.getTeam();
```

[두 객체 사이에 연관 관계가 없을 때](#anchor1)와 달리, `Team` 객체 자체를 인자로 넘겨 저장할 수 있고, **조회를 할 때 DB에 접근하지 않고 `Member` 객체의 참조 변수를 이용해 접근(객체 그래프 탐색)**할 수 있게 된다.

> 새로운 `Team` 객체를 생성해서 `Member` 객체의 `team` 값을 변경해주면 외래 키의 값이 새로운 `Team` 객체 값으로 바뀌게 되면서 DB의 연관 관계도 함께 바뀌게 된다.
{: .prompt-info }

### 양방향 매핑

![04-two-way-relationship-design](/assets/img/posts/api/jpa/relationship-mapping/04-two-way-relationship-design.jpg)
*양방향 객체 연관 관계가 있는 설계 방식*

객체 관계에서 양방향 객체 연관 관계가 성립되려면 각 객체는 서로의 객체 자체를 직접적으로 참조하고 있어야 하지만, 외래 키로 연결된 DB의 테이블 관계에서는 기존의 외래 키를 이용하여 단방향 또는 양방향 연관 관계를 모두 설계할 수 있다.

**즉, 단방향 연관 관계를 양방향 연관 관계로 수정할 때 DB의 테이블은 외래 키를 통해 이미 연결되어 있기 때문에 단방향이든 양방향이든 기존의 테이블 구조를 유지할 수 있어 설계가 수정될 필요가 없지만 객체 관계에서는 수정이 필요하다는 것이다.**

```java
@Entity
public class Member {

    @Id @GeneratedValue
    private Long id;

    @Column(name = "USERNAME")
    private String name;

    private int age;

//  @Column(name = "TEAM_ID")
//  private Long teamId;

    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;

    // ...
}

@Entity
public class Team {

    @Id @GeneratedValue
    private Long id;

    private String name;

    @OneToMany(MappedBy = "team")
    List<Member> members = new ArrayList<>();

    // ...
}
```

`Team` 클래스에 컬렉션을 추가하고, 추가한 컬렉션에 `@OneToMany` 어노테이션을 사용하여 일대다(1:N) 연관 관계임을 표현한다. 이때 `mappedBy` 속성을 사용하여 양방향 관계임을 명시하고, `team`을 지정하여 `Team` 객체의 `team` 필드와 매핑됨을 나타낸다.

```java
// 팀 저장
Team team = new Team();
team.setName("TeamA");
em.persist(team);

// 회원 저장
Member member = new Member();
member.setName("member1");
member.setTeam(team); // 단방향 연관 관계 설정, 참조 저장
em.persist(member);

// 조회
Member findMember = em.find(Member.class, member.getId());

// 참조를 사용해서 연관 관계 조회
Team findTeam = findMember.getTeam();

List<Member> members = findTeam.getMembers();

for (Member member1 : members) {
    System.out.println("member1: " + member1);
}
```

객체를 양방향 연관 관계로 설계해놓았기 때문에 `Team` 객체에서도 `Members` 객체에 역방향으로 접근할 수 있게 된다. 

#### 양방향 연관 관계의 주인(Owner)

![04-two-way-relationship-design](/assets/img/posts/api/jpa/relationship-mapping/04-two-way-relationship-design.jpg)
*양방향 객체 연관 관계가 있는 설계 방식*

위의 양방향 객체 연관 관계에서 `Member` 객체는 `Team` 객체의 참조 변수 `team`을 통해 `Team` 객체에 접근할 수 있다. `Team` 객체 또한 `Member` 타입 컬렉션 객체의 참조 변수 `members`를 통해 `Member` 객체에 접근할 수 있다. 

- A -> B
	+ `a.getB()`
- B -> A
	+ `b.getA()`

객체 관계에서 양방향처럼 보이는 이 관계는 사실 두 개의 방향이 서로를 향하고 있기 때문에 양방향 연관 관계와 같은 효과를 내는 것이다.

```sql
SELECT *
  FROM MEMBER M
  JOIN TEAM T
    ON M.TEAM_ID = T.TEAM_ID;

SELECT *
  FROM TEAM T
  JOIN MEMBER M
    ON T.TEAM_ID = M.TEAM_ID;
```

반면, 테이블 관계에서는 외래 키 하나로 양방향 연관 관계를 갖는다. 테이블 관계에서의 방향과 객체 관계에서의 방향의 개념이 다르다는 것을 알 수 있다.

![05-two-way-relationship-mapping](/assets/img/posts/api/jpa/relationship-mapping/05-two-way-relationship-mapping.jpg)
*양방향 객체 연관 관계 매핑*

`Member` 객체에서 `Team` 객체의 `team` 값을 변경해도 연관 관계가 바뀌게 되고, `Team` 객체에서 `Member` 타입 컬렉션 객체의 `members` 값을 변경해도 연관 관계가 바뀐다.

***그렇다면 `Member` 객체와 `Team` 객체의 연관 관계가 서로 일치하지 않을 경우 어느 연관 관계를 우선해서 참조해야 할까?***

JPA에서는 이러한 문제를 해결하기 위해 연관 관계를 설정할 때, **두 객체 중 하나를 주인으로 지정하여 주인 객체를 통해 연관된 객체를 수정 및 삭제**할 수 있게 한다. **비주인 객체는 주인 객체를 통해 조회만 가능하며, 자동으로 변경사항을 감지하여 업데이트하지 않는다.** 주인 객체를 변경하는 쪽이 데이터베이스에 반영되는데, 이를 통해 연관된 객체의 변경사항이 데이터베이스에 일관되게 반영될 수 있도록 한다.

#### 양방향 매핑 규칙

![06-two-way-relationship-mapping-owner](/assets/img/posts/api/jpa/relationship-mapping/06-two-way-relationship-mapping-owner.jpg)

- **외래 키를 가지고 있는 객체를 연관 관계의 주인(Owner)으로 지정**
- 비주인(Inverse)이 아니면 `mappedBy` 속성으로 주인 명시
- **연관 관계의 주인만이 외래 키를 관리(등록, 수정)**
- **비주인은 조회만 가능**
- 주인은 `mappedBy` 속성 사용 X
- 단방향으로만 설계하고 필요시 양방향으로 확장
	+ 단순한 설계로 출발하여 코드가 간결
		* 설계 초기부터 양방향으로 설정하면 코드가 복잡해지고 유지보수가 어려움
		* 필요에 따라 양방향 관계를 추가함으로써 유연성 확보
	+ 지연로딩을 쉽게 적용할 수 있어 성능 최적화
	+ 의존성 최소화
	+ 양방향으로 확장할 때 DB 수정없이 객체 관계만 수정하면 됨

#### 예시

```java
Team team = new Team(); // 비주인
team.setName("TeamA";
em.eprsist(team);

Member member = new Member(); // 주인
member.setName("member1");

// 연관 관계의 비주인에 값 설정
team.getMembers().add(member); // FK NULL

em.persist(member);
```

| ID | AGE | USERNAME | TEAM_ID(FK) |
|:---:|:------:|:--------------:|:---------------:|
|  1 |   0    |     hello      |    **null**    |

```java
Team team = new Team(); // 비주인
team.setName("TeamA";
em.eprsist(team);

Member member = new Member(); // 주인
member.setName("member1");

// 연관 관계의 주인에 값 설정
member.setTeam(team); // FK NOT NULL

em.persist(member);
```

| ID | AGE | USERNAME | TEAM_ID(FK) |
|:---:|:------:|:--------------:|:---------------:|
|  1 |   0    |     hello      |      **1**     |

> 실무에서는 연관 관계의 주인, 비주인 객체에 참조 객체를 각각 모두 넣는 것을 권장한다.
{: .prompt-tip }

## 연관 관계 매핑 어노테이션

- `@ManyToOne`: 다대일(N:1) 관계
- `@OneToMany`: 일대다(1:N) 관계
- `@OneToOne`: 일대일(1:1) 관계
- `@ManyToMany`: 다대다(N:N) 관계
	+ 권장되는 방법이 아니며, 대신에 일대다(1:N):다대일(N:1) 관계로 대체
- `@JoinColumn`, `@JoinTable`

## 상속 관계 매핑 어노테이션

- `@Inheritance`
- `@DiscriminatorColumn`
- `@DiscriminatorValue`
- `@MappedSuperclass`
	+ 매핑 속성만 상속

## 복합키 어노테이션

- `@IdClass`
- `@EmbeddeId`
- `@Embeddable`
- `@MapsId`

## 참고자료

- [김영한, "[토크ON세미나] JPA 프로그래밍 기본기 다지기 4강 - 연관관계 매핑 | T아카데미", 
SKplanet Tacademy, 2018-12-21](https://www.youtube.com/watch?v=bEtTpCviSc4){: target="_blank" }
- [김영한, "[토크ON세미나] JPA 프로그래밍 기본기 다지기 5강 - 양방향 매핑 | T아카데미", 
SKplanet Tacademy, 2018-12-21](https://www.youtube.com/watch?app=desktop&v=0zTtkIYMOIw){: target="_blank" }