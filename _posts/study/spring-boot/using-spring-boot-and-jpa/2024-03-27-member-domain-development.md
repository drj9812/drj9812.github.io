---
title: "[Spring Boot]스프링 부트와 JPA 활용1 - 회원 도메인 개발"
categories: [Study, Spring Boot]
tags: [Java, 자바, Spring Boot, 스프링 부트, JPA, 인프런, Inflearn, 김영한]
image:
  path: /assets/img/posts/study/spring-boot/using-spring-boot-and-jpa/01-using-spring-boot-and-jpa-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 김영한 - 실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발
---

# 스프링 부트와 JPA 활용1 - 회원 도메인 개발

## 커리큘럼

1. [[Spring Boot]스프링 부트와 JPA 활용1 - 프로젝트 환경 설정](https://drj9812.github.io/posts/project-configuration/){: target="_blank" }
2. [[Spring Boot]스프링 부트와 JPA 활용1 - 도메인 분석 설계](https://drj9812.github.io/posts/domain-analysis-design/){: target="_blank" }
3. [**[Spring Boot]스프링 부트와 JPA 활용1 - 회원 도메인 개발**](https://drj9812.github.io/posts/member-domain-development){: target="_blank" }
4. [[Spring Boot]스프링 부트와 JPA 활용1 - 상품 도메인 개발](https://drj9812.github.io/posts/item-domain-development){: target="_blank" }
5. [[Spring Boot]스프링 부트와 JPA 활용1 - 주문 도메인 개발](https://drj9812.github.io/posts/order-domain-development){: target="_blank" }
6. [[Spring Boot]스프링 부트와 JPA 활용1 - 웹 계층 개발](https://drj9812.github.io/posts/web-layer-development){: target="_blank" }

## 회원 Repository 개발

### repository.memberRepository.java

```java
package jpabook.jpashop.repository;

import java.util.List;

import org.springframework.stereotype.Repository;

import jakarta.persistence.EntityManager;
import jpabook.jpashop.domain.Member;

@Repository
@RequiredArgsConstructor
public class MemberRepository {

    private final EntityManager em;
	
    public void save(Member member) {
        em.persist(member);
    }
	
    public Member findOne(Long id) {
        return em.find(Member.class, id);
    }
	
    public List<Member> findAll() {
        return em.createQuery("SELECT m FROM Member AS m", Member.class)
								.getResultList();
    }
	
    public List<Member> findByName(String name) {
        return em.createQuery("SELECT m FROM Member AS m WHERE m.name = :name", Member.class)
				.setParameter("name", name)
				.getResultList();
    }
}
```

- `@Repository`
	+ 스프링 프레임워크의 컴포넌트 스캔의 대상이 되는 어노테이션
	+ 데이터 액세스 계층의 빈으로 등록되는 클래스를 표시
	+ DAO(Data Access Object) 패턴을 구현하는 클래스에 지정
	+ Spring Data JPA에서 제공하는 Reposiotry 인터페이스를 상속한다면 명시하지 않아도 됨

> `@SpringBootApplication` 어노테이션이 붙은 클래스의 경우 스프링 부트 애플리케이션의 진입점을 나타내며, 이 어노테이션은 여러 다른 어노테이션들을 포함하고 있다. 그 중에는 `@ComponentScan` 어노테이션도 포함되어 있는데, `@ComponentScan` 어노테이션은 `@SpringBootApplication` 어노테이션이 붙은 클래스의 패키지와 그 하위 패키지에 있는 컴포넌트들을 스캔하여 빈으로 등록한다.
{: .prompt-info }

> `@PersistenceUnit` 어노테이션을 사용하면 `EntityManagerFactory`를 직접 주입받을 수 있다.
{: .prompt-info }

>  일반적인 JPA에서는 `@PersistenceContext` 어노테이션을 사용하여 `EntityManager`를 주입받지만 Spring Data JPA를 사용할 때는 `@Autowired` 어노테이션 또는 Lombok을 사용한 생성자 주입을 통해서도 `EntityManager`를 주입받을 수 있다.
{: .prompt-info }

## 회원 Service 개발

### service.memberService.java

```java
package jpabook.jpashop.service;

import java.util.List;

import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import jpabook.jpashop.domain.Member;
import jpabook.jpashop.repository.MemberRepository;
import lombok.RequiredArgsConstructor;

@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class MemberService {
	
    private final MemberRepository memberRepository;
	
    /**
     * 회원 가입
     */
    @Transactional
    public Long join(Member member) {
        validateDuplicateMember(member); // 중복 회원 검증
	memberRepository.save(member);
		
	return member.getId(); // 영속성 컨텍스트에 Entity가 있다는 걸 보장
    }
	
    private void validateDuplicateMember(Member member) {
        List<Member> findMembers = memberRepository.findByName(member.getName());
		
	if (!findMembers.isEmpty()) {
	    throw new IllegalStateException("이미 존재하는 회원입니다.");
	}
    }
	
    // 회원 전체 조회
    public List<Member> findMembers() {
        return memberRepository.findAll();
    }
	
    public Member findMember(Long memberId) {
        return memberRepository.findOne(memberId);
    }
}
```

- `@Transactional`
	+ 스프링 프레임워크에서 제공하는 트랜잭션 관리를 위한 어노테이션
	+ 메서드나 클래스에 적용하여 해당 메서드나 클래스의 실행을 트랜잭션 내에서 처리하도록 지시
		* 클래스 레벨에 @Transactional 어노테이션을 선언할 경우, 스프링의 트랜잭션 관리는 프록시 기반의 AOP를 사용하므로 `public` 메서드에만 적용
			* AspectJ를 사용하면 `public` 미만의 메서드에도 `@Transactional` 어노테이션을 적용시킬 수 있음
	+ `readOnly = true`
		* 트랜잭션이 읽기 전용(read-only)으로 설정되어야 함을 나타내며, 영속성 컨텍스트를 flush하지 않음
		* **해당 트랜잭션 내에서는 데이터를 읽기만 가능하고, 데이터를 수정, 추가 또는 삭제하는 작업은 허용되지 않음**
		* DB에 대한 조회 작업을 최적화
		* default false

> Jakarta EE(Java EE)와 스프링 프레임워크 모두 트랜잭션 관리를 위한 `@Transactional` 어노테이션을 제공하지만, 프로젝트를 스프링 기반으로 만들었기 때문에 스프링 프레임워크에서 제공하는 `@Transactional` 어노테이션을 사용했다.
{: .prompt-info }

> `readOnly = true` 옵션을 사용하면 DB에 대한 읽기 작업을 최적화할 수 있다. 읽기 전용 트랜잭션에서는 DB의 일관성을 유지하는 데 필요한 추가적인 락(lock)을 얻지 않으므로 트랜잭션 처리 시간이 단축될 수 있다. 또한, 해당 트랜잭션 내에서는 Dirty Checking 같은 부가적인 작업이 필요하지 않으므로 성능이 향상될 수 있다.
{: .prompt-tip }

> 실무에서는 검증 로직(`validateDuplicateMember()`)이 있어도 multi-thread 상황을 고려해서 회원 테이블의 회원명 컬럼에 유니크 제약 조건을 추가하는 것이 안전하다.
{: .prompt-tip }

## 회원 기능 테스트

```java
package jpabook.jpashop.service;

import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.annotation.Rollback;
import org.springframework.transaction.annotation.Transactional;

import jpabook.jpashop.domain.Member;

@SpringBootTest
@Transactional
public class MemberServiceTest {

    @Autowired
    private jpabook.jpashop.repository.MemberRepository memberRepository;
	
    @Autowired
    private MemberService memberService;

    @Test
//  @Rollback(false)
    public void 회원가입() throws Exception {
    	// given
	Member member = new Member();
	member.setName("kim");
		
	// when
	Long savedId = memberService.join(member);
		
	// then
	Assertions.assertThat(member).isEqualTo(memberRepository.findOne(savedId));
    }
	
    @Test
//  @Rollback(false)
    public void 중복_회원_예외() throws Exception {
	// given
	Member member1 = new Member();
	member1.setName("kim");
		
	Member member2 = new Member();
	member2.setName("kim");
		
	// when
	memberService.join(member1);
		
	try
	    memberService.join(member2);
			
	} catch (IllegalStateException e) {
	    return;
	}
		
	// then
	Assertions.fail("예외가 발생해야 한다.");
    }
}
```

- `@SpringBootTest`
	+ 스프링 애플리케이션 컨텍스트를 로드하여, 테스트 클래스 내에서 스프링 빈 객체를 주입하여 사용할 수 있도록 함
- `@Transactional`
	+ `@Transactional` 어노테이션이 테스트 클래스에서 사용되면 테스트가 끝난 후, 트랜잭션이 롤백됨
		* flush하지 않으므로 `INSERT` 쿼리가 실행되지 않음
		* `EntityManager`를 통해 강제로 flush함으로써 `INSERT` 쿼리의 실행을 강제할 수 있음
		* `@Rollback()` 어노테이션을 사용하여 트랜잭션이 롤백되지 않도록 설정할 수 있음

## 다음 글

- [[Spring Boot]스프링 부트와 JPA 활용1 - 상품 도메인 개발](https://drj9812.github.io/posts/item-domain-development){: target="_blank" }

## 참고자료

- [김영한, "실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발", Inflearn, Date unknown](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1){: target="_blank" }