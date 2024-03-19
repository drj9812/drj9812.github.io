---
title: "[JPA]연관 관계 매핑"
categories: [Study, JPA]
tags: [JPA, 연관 관계, 일대다 관계, 다대일 관계, 양방향 관계, 단방향 관계, mapping, 매핑]
---

# 연관 관계 매핑

<a id=anchor1></a>

## 연관 관계의 중요성

![01-no-relationship-design](/assets/img/posts/study/jpa/relationship-mapping/01-no-relationship-design.jpg)
*객체 연관 관계가 없는 설계 방식*

위 사진은 연관 관계를 고려하지 않는 설계 방식을 나타낸 것이다. 즉, `Member` 객체는 `Team` 객체의 외래 키(`teamId`)를 가지고 있을 뿐 `Team` 객체를 참조하고 있지 않다. 

이러한 설계 방식은 객체를 테이블에 맞추어 데이터 중심으로 모델링한 것으로, 주로 관계형 DB 모델링의 관점에서 접근했기 때문에 발생한다.

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

[사진](#anchor1)과 같이 참조 대신에 외래 키를 그대로 사용한다고 가정해보자.

<a id="anchor2"></a>

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

DB의 테이블에서는 연관된 데이터를 `JOIN`을 통해 외래 키로 찾을 수 있지만, 객체는 참조를 통해 연관된 객체를 찾는다. 위 코드에서도 알 수 있듯이 두 객체 사이에 연관 관계가 없다면 각 객체를 조회하기 위해 각각의 객체를 조회해야하는 문제점이 발생한다. 테이블과 객체 사이에는 이런 큰 간격이 있다.

객체를 테이블에 맞추어 데이터 중심으로 모델링하게 되면 협력 관계를 만들 수 없고, 이러한 설계 방식은 객체 지향적이지 않다는 것이다.

## 연관 관계 매핑 이론

### 단방향 매핑

![02-one-sided-relationship-design](/assets/img/posts/study/jpa/relationship-mapping/02-one-sided-relationship-design.jpg)
*단방향 객체 연관 관계가 있는 설계 방식*

반면에 위 사진은 연관 관계를 고려한, 객체 지향적인 설계 방식이다. `Member`는 외래 키(`teamId`)를 참조하는 것이 아닌, `Team` 객체 자체를 직접적으로 참조하고 있다. 객체 연관 관계만 바뀌었을 뿐 테이블 연관 관계는 바뀌지 않았다.

이때 `Member` 객체는 `Team` 객체를 참조할 수 있지만, `Team` 객체는 `Member` 객체를 참조할 수 없는 구조이기 때문에 단방향 관계다.

![03-one-sided-relationship-mapping](/assets/img/posts/study/jpa/relationship-mapping/03-one-sided-relationship-mapping.jpg)
*ORM 매핑*

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

[두 객체 사이에 연관 관계가 없을 때](#anchor2)와 달리, `Team` 객체 자체를 인자로 넘겨 저장할 수 있고, 조회를 할 때도 DB에 접근하지 않고 `Member` 객체의 참조 변수를 이용해 접근(객체 그래프 탐색)할 수 있게 된다.

> 새로운 `Team` 객체를 생성해서 `Member` 객체의 `team` 값을 변경해주면 외래 키의 값이 새로운 `Team` 객체 값으로 바뀌게 되면서 DB의 연관 관계도 함께 바뀌게 된다.
{: .prompt-info }

### 양방향 매핑

![04-two-way-relationship-design](/assets/img/posts/study/jpa/relationship-mapping/04-two-way-relationship-design.jpg)
*양방향 객체 연관 관계가 있는 설계 방식*

객체 관계에서 양방향 객체 연관 관계가 성립되려면 각 객체는 서로의 객체 자체를 직접적으로 참조하고 있어야 하지만, 외래 키로 연결된 DB의 테이블 관계에서는 기존의 외래 키를 이용하여 단방향 또는 양방향 연관 관계를 모두 설계할 수 있다.

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

## 참고 자료

- [김영한, "[토크ON세미나] JPA 프로그래밍 기본기 다지기 4강 - 연관관계 매핑 | T아카데미", 
SKplanet Tacademy, 2018-12-21](https://www.youtube.com/watch?v=bEtTpCviSc4){: target="_blank" }
- [김영한, "[토크ON세미나] JPA 프로그래밍 기본기 다지기 5강 - 양방향 매핑 | T아카데미", 
SKplanet Tacademy, 2018-12-21](https://www.youtube.com/watch?app=desktop&v=0zTtkIYMOIw){: target="_blank" }