---
title: "[JPA]기본 키(Primary Key) 생성 전략"
categories: [공부, JPA]
tags: [JPA, Primary Key, 기본 키]
---

# 기본 키(Primary Key) 생성 전략

## 참조

- [[JPA]JPA](https://drj9812.github.io/posts/jpa/){: target="_blank" }

## 기본 키를 생성하는 방법

### 직접 할당 방식

- 기본 키를 애플리케이션에서 자체적으로 생성하여 할당하는 방식
- 애플리케이션에서 별도의 키 생성 규칙을 정의하여 사용해야 중복 문제가 발생하지 않음

### Identity

- `@GeneratedValue(stategy = GenerationType.IDENTITY)`
- PK 생성을 DB에 위임하는 전략
- 컬럼에 `AUTO_INCREMENT` 기능을 활용하여 값을 사용
- **Identity 전략은 `persist()` 메서드가 호출되는 시점에 Insert 쿼리가 실행되며 식별자를 조회**
	+ 영속성 컨텍스트에서 영속 객체를 관리하려면 1차 캐시에 식별자를 저장해야되는데, PK 생성을 DB에서 담당하므로 쿼리가 먼저 실행되어야 함
	+ 일반적으로 다른 전략 방식은 커밋 시점에 쿼리 실행

### Sequence

- `@GeneratedValue(strategy = GenerationType.SEQUENCE)`
- DB의 시퀀스를 이용하는 전략
- `@SequenceGenerator(name="sequence_generator", sequenceName = "my_seq", allocationSize = 1)`
	+ 시퀀스 전략을 사용할 때는 `Entity` 클래스나 식별자('@Id`)에 `@SequenceGenerator` 어노테이션을 명시해줘야 함
	+ `name`
		* 시퀀스 생성기의 이름
		* `@GeneratedValue` 어노테이션의 generator 값으로 사용
		* 필수 속성
	+ `sequenceName`
		* 식별자를 생성할 때 사용할 Sequence 이름을 지정
	+ allocationSize
		* 시퀀스에서 읽어온 값을 기준으로 몇 개의 식별자를 생성할지 결정
		* default 50

#### Sequence란?

- Unique 값을 생성해주는 객체
- 일반적으로 Primary Key를 생성하기 위해 사용됨
- 지원하는 DB
	+ Oracle
	+ PostgresSQL
	+ H2
	+ MariaDB

### Table

- `@GeneratedValue(strategy = GenerationType.TABLE)`
- DB의 시퀀스 역할을 수행하는 테이블을 생성하여 활용하는 전략
- 시퀀스를 지원하지 않는 DB를 사용할 때 이용
- 목적에 맞게 최적화된 테이블이 아니기 때문에 성능상 이슈가 발생할 수 있음
- `@TableGeneration(name = "id_generator", table = "id_gen", pkColumnName = "entity", pkColumnValue = "table_gen", valueColumnName = "next_id", initialValue = 0, allocationSize = 1)`
	+ 테이블 전략을 사용할 때는 `Entity` 클래스나 식별자에 `@TableGeneration` 어노테이션을 명시해줘야 함
	+ name
		* 테이블 생성기의 이름
		* `@GeneratedValue` 어노테이션의 generator 값으로 사용
		* 필수 속성
	+ table
		* 식별자를 생성할 때 사용할 테이블 이름을 지정
	+ pkColumnName
		* Id 생성용 테이블의 PK 컬럼을 지정
	+ pkColumnValue
		* 테이블 생성기가 PK 컬럼에 사용할 값을 지정
		* 각 테이블 생성기마다 다른 값을 사용
	+ valueColumnName
		* 생성할 Id 값을 갖는 컬럼 이름
	+ initialValue
		* Id 초기값을 지정
		* Id 생성용 테이블에 해당 레코드가 없을 경우 이 속성의 값으로 생성
		* default 0
	+ allocationSize
		* Id의 할당 크기를 지정

### Auto

- `@GeneratedValue(strategy = GenerationType.Auto)`
- 연동되어 있는 DB의 종류에 따라 앞서 소개한 전략을 자동으로 채택
- 생성을 전적으로 애플리케이션에게 위임하기 때문에 권장되는 전략은 아님

## 실행 예시

### 구조 및 코드

![01-id_generation-project-structure](/assets/img/posts/study/jpa/primary-key-generation-strategy/01-id_generation-project-structure.jpg)
*구조*

#### DirectEntity

```java
package studio.aroundhub.id_generation.entity;

import java.time.LocalDateTime;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "direct")
public class DirectEntity {
    @Id
    private Long number;
    private String name;

    @Column(name = "created_at")
    private LocalDateTime createdAt;

    @Column(name = "updated_at")
    private LocalDateTime updatedAt;

    public DirectEntity(){}
    
    public DirectEntity(Long number, String name,
    				    LocalDateTime createdAt,
    				    LocalDateTime updatedAt) {
    	this.number = number;
    	this.name = name;
    	this.createdAt = createdAt;
    	this.updatedAt = updatedAt;
    }
    
    public Long getNumber() {
        return number;
    }

    public String getName() {
        return name;
    }

    public LocalDateTime getCreatedAt() {
        return createdAt;
    }

    public LocalDateTime getUpdatedAt() {
        return updatedAt;
    }
}
```

#### IdentityEntity

```java
package studio.aroundhub.id_generation.entity;

import java.time.LocalDateTime;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "identity")
public class IdentityEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long number;
    private String name;

    @Column(name = "created_at")
    private LocalDateTime createdAt;

    @Column(name = "updated_at")
    private LocalDateTime updatedAt;
    
    public IdentityEntity(){}
    
    public IdentityEntity(String name,
    			   		  LocalDateTime createdAt,
    			   		  LocalDateTime updatedAt) {
    	this.name = name;
    	this.createdAt = createdAt;
    	this.updatedAt = updatedAt;
    }

    public Long getNumber() {
        return number;
    }

    public String getName() {
        return name;
    }

    public LocalDateTime getCreatedAt() {
        return createdAt;
    }

    public LocalDateTime getUpdatedAt() {
        return updatedAt;
    }
}
```

#### SequenceEntity

```java
package studio.aroundhub.id_generation.entity;

import java.time.LocalDateTime;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.SequenceGenerator;
import javax.persistence.Table;

@Entity
@Table(name = "sequence")
public class SequenceEntity {
    /**
     * allocationSize를 설정하지 않았을 경우 아래와 같은 방식으로 식별자를 생성
     * <p>
     * 1. 최초에 DB 시퀀스에서 값을 구함. 이 값을 식별자 구간의 시작 값으로 사용
     * 2. 한 번 더 DB 시퀀스에 값을 구함. 이를 식별자 구간의 끝 값으로 사용
     * 3. 구간의 시작 값부터 끝 값까지 순차적으로 엔티티의 식별 값으로 사용.
     *      이때 메모리에 순차적으로 증가한 값을 보관 --> 이 과정 때문에 allocationSize를 1로 설정하지 않았을 경우, 여러 JVM이 가동되는 가정하에 충돌이 발하게됨
     * 4. 구간의 끝에 다다르면 DB 시퀀스에 값을 구함.
     *      시퀀스에서 구한 값을 다음 식별자 구간의 끝 값으로 사용한다. 구간 끝 값에서 (allocationSize -1)를 뺀 값을 식별자 구간의 시작 값으로 사용
     * 5. 과정 3과 4를 반복
     */
    /*
    name : 시퀀스 생성기의 이름을 지정. 이 이름은 @GeneratedValue 에서 사용됨
    sequenceName : 식별자(Id)를 생성할 때 사용할 Sequence 이름을 지정
    initialValue : DDL을 통해 데이터베이스를 생성할 때 사용되는 값으로, 최초 설정되는 값을 정의(default: 1)
    allocationSize : 시퀀스에서 읽어온 값을 기준으로 몇 개의 식별자를 생성할지 결정. 값은 1로 설정해야 함 (default: 50)
     */
    @Id
    @GeneratedValue(generator = "sequence_generator")
    @SequenceGenerator(name = "sequence_generator",
    			       sequenceName = "my_seq",
    			       allocationSize = 1)
    private Long id;
    private String name;

    @Column(name = "created_at")
    private LocalDateTime createdAt;

    @Column(name = "updated_at")
    private LocalDateTime updatedAt;

    public SequenceEntity() {}

    public SequenceEntity(String name,
    		LocalDateTime createdAt,
    		LocalDateTime updatedAt) {
    	this.name = name;
    	this.createdAt = createdAt;
    	this.updatedAt = updatedAt;
    }
    
    
    public Long getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public LocalDateTime getCreatedAt() {
        return createdAt;
    }

    public LocalDateTime getUpdatedAt() {
        return updatedAt;
    }
}
```

#### TableEntity

```java
package studio.aroundhub.id_generation.entity;

import java.time.LocalDateTime;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.Table;
import javax.persistence.TableGenerator;

@Entity
@Table(name = "table_gen")
public class TableEntity {

    /*
    TableGenerator Comments

    name : 테이블 생성기의 이름. @GeneratedValue의 generator 값으로 사용
    table : Id를 생성할 때 사용할 테이블
    pkColumnName : Id 생성용 테이블의 PK 칼럼을 지정
    pkColumnValue : 테이블 생성기가 PK 칼럼에 사용할 값을 지정. 각 TableGenerator 마다 다른 값을 사용.
    valueColumnName : 생성할 Id를 갖는 칼럼을 지정
    initialValue : Id 초기값을 지정. id 생성용 테이블에 해당 레코드가 없을 경우 이 속성의 값으로 생성
    allocationSize : Id의 할당 크기를 지정.
     */
    /*
    TableGenerator는 아래와 같은 과정을 통해 Id값을 생성함

    1. id_generator 테이블에 Entity 칼럼 값이 table인 레코드를 구함
      - 레코드가 존재하지 않으면 initialValue인 0에 1을 더한 값을 id 값으로 사용하고 이 값을 id_generator 테이블에 삽입
      - 기존에 레코드가 존재할 경우에는 next_id 값을 가져와서 사용
    2. 다음 Id 값(table, Id + 1)을 id_generator 테이블에 반영
     */
    @Id
    @TableGenerator(name = "id_generator",
        		    table = "id_gen",
        		    pkColumnName = "entity",
        		    pkColumnValue = "table_gen",
        		    valueColumnName = "next_id",
        		    initialValue = 0,
        		    allocationSize = 1)
    @GeneratedValue(generator = "id_generator")
    private Long number;
    private String name;

    @Column(name = "created_at")
    private LocalDateTime createdAt;

    @Column(name = "updated_at")
    private LocalDateTime updatedAt;

    public TableEntity() {}
    
    public TableEntity(String name,
    		LocalDateTime createdAt,
    		LocalDateTime updatedAt) {
    	this.name = name;
    	this.createdAt = createdAt;
    	this.updatedAt = updatedAt;
    }
    
    public Long getNumber() {
        return number;
    }

    public String getName() {
        return name;
    }

    public LocalDateTime getCreatedAt() {
        return createdAt;
    }

    public LocalDateTime getUpdatedAt() {
        return updatedAt;
    }
}
```

#### CEntityManagerFactory

```java
package studio.aroundhub.id_generation.factory;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;

public class CEntityManagerFactory {

    private static EntityManagerFactory entityManagerFactory;

    /**
     * EntityManagerFactory는 EntityManager를 생성하기 위한 팩토리 인터페이스로 persistence 단위별로 팩토리를 생성
     */
    public static void initialization(){
        entityManagerFactory = Persistence.createEntityManagerFactory("id_generation");
    }

    /**
     * EntityManager 객체를 생성
     * EntityManager 는 Persistence Context와 Entity를 관리
     *
     * @return EntityManager 객체
     */
    public static EntityManager createEntityManger(){
        return entityManagerFactory.createEntityManager();
    }

    /**
     * EntityManagerFactory 객체 종료
     */
    public static void close(){
        entityManagerFactory.close();
    }

}
```

#### IdGenerationService

```java
package studio.aroundhub.id_generation.service;

import java.util.Optional;
import studio.aroundhub.id_generation.entity.DirectEntity;
import studio.aroundhub.id_generation.entity.IdentityEntity;
import studio.aroundhub.id_generation.entity.TableEntity;

public interface IdGenerationService {
    void insertDirectEntity(String name);

    Optional<DirectEntity> selectDirectEntity(String id);

    void insertIdentityEntity(String name);

    Optional<IdentityEntity> selectIdentityEntity(Long id);

    void insertTableEntity(String name);

    Optional<TableEntity> selectTableEntity(Long id);

    void insertSequenceEntity(String name);

    Optional<TableEntity> selectSequenceEntity(Long id);
}
```

#### IdGenerationServiceImpl

```java
package studio.aroundhub.id_generation.service.impl;

import java.time.LocalDateTime;
import java.util.Optional;
import javax.persistence.EntityManager;
import studio.aroundhub.id_generation.entity.DirectEntity;
import studio.aroundhub.id_generation.entity.IdentityEntity;
import studio.aroundhub.id_generation.entity.SequenceEntity;
import studio.aroundhub.id_generation.entity.TableEntity;
import studio.aroundhub.id_generation.factory.CEntityManagerFactory;
import studio.aroundhub.id_generation.service.IdGenerationService;

public class IdGenerationServiceImpl implements IdGenerationService {
    static long num = 0;

    private static Long createNumber() {
        return num++;
    }

    @Override
    public void insertDirectEntity(String name) {
        EntityManager entityManager = CEntityManagerFactory.createEntityManger();

        // EntityTransaction 으로 객체를 생성해서 트랜잭션 관리를 해도 되지만 EntityManager 만으로도 사용할 수 있음
        entityManager.getTransaction().begin();

        try {
            System.out.println("CheckPoint 1");
            
            Long number = createNumber();
            DirectEntity directEntity = new DirectEntity(number, name,
            											 LocalDateTime.now(),
            											 LocalDateTime.now());

            System.out.println("CheckPoint 2");
            
            // Persistence Context 에 객체 추가
            entityManager.persist(directEntity);

            System.out.println("CheckPoint 3");
            
            // 실제 DB 적용
            entityManager.getTransaction().commit();

            System.out.println("CheckPoint 4");

        } catch (Exception e) {
            entityManager.getTransaction().rollback();
            e.printStackTrace();

        } finally {
            entityManager.close();
        }
    }

    @Override
    public Optional<DirectEntity> selectDirectEntity(String id) {
        return Optional.empty();
    }

    @Override
    public void insertIdentityEntity(String name) {
        EntityManager entityManager = CEntityManagerFactory.createEntityManger();

        // EntityTransaction 으로 객체를 생성해서 트랜잭션 관리를 해도 되지만 EntityManager 만으로도 사용할 수 있음
        entityManager.getTransaction().begin();

        try {
            System.out.println("CheckPoint 1");

            IdentityEntity identityEntity = new IdentityEntity(name,
            												   LocalDateTime.now(),
            												   LocalDateTime.now());

            System.out.println("CheckPoint 2");
            
            // Persistence Context 에 객체 추가
            entityManager.persist(identityEntity); // 이 단계에서 Id 값이 영속성 컨텍스트에 반영됨

            System.out.println("CheckPoint 3");
            System.out.println("id value : " + identityEntity.getNumber());

            // 실제 DB 적용
            entityManager.getTransaction().commit();

            System.out.println("CheckPoint 4");
            
        } catch (Exception e) {
            entityManager.getTransaction().rollback();
            e.printStackTrace();

        } finally {
            entityManager.close();
        }
    }

    @Override
    public Optional<IdentityEntity> selectIdentityEntity(Long id) {
        return Optional.empty();
    }

    @Override
    public void insertTableEntity(String name) {
        EntityManager entityManager = CEntityManagerFactory.createEntityManger();

        // EntityTransaction 으로 객체를 생성해서 트랜잭션 관리를 해도 되지만 EntityManager 만으로도 사용할 수 있음
        entityManager.getTransaction().begin();

        try {
            System.out.println("CheckPoint 1");
            TableEntity tableEntity = new TableEntity(name,
            										  LocalDateTime.now(),
                                                      LocalDateTime.now());

            System.out.println("CheckPoint 2");
            
            // Persistence Context 에 객체 추가
            entityManager.persist(tableEntity); // 이 단계에서 Id 값이 영속성 컨텍스트에 반영됨

            System.out.println("CheckPoint 3");
            System.out.println("id value : " + tableEntity.getNumber());

            // 실제 DB 적용
            entityManager.getTransaction().commit();

            System.out.println("CheckPoint 4");

        } catch (Exception e) {
            entityManager.getTransaction().rollback();
            e.printStackTrace();

        } finally {
            entityManager.close();
        }
    }

    @Override
    public Optional<TableEntity> selectTableEntity(Long id) {
        return Optional.empty();
    }

    @Override
    public void insertSequenceEntity(String name) {
        EntityManager entityManager = CEntityManagerFactory.createEntityManger();

        // EntityTransaction 으로 객체를 생성해서 트랜잭션 관리를 해도 되지만 EntityManager 만으로도 사용할 수 있음
        entityManager.getTransaction().begin();

        try {
            System.out.println("CheckPoint 1");
            SequenceEntity sequenceEntity = new SequenceEntity(name,
            						                           LocalDateTime.now(),
                                                               LocalDateTime.now());

            System.out.println("CheckPoint 2");
            
            // Persistence Context 에 객체 추가
            entityManager.persist(sequenceEntity); // 이 단계에서 Id 값이 영속성 컨텍스트에 반영됨

            System.out.println("CheckPoint 3");
            System.out.println("id value : " + sequenceEntity.getId());

            // 실제 DB 적용
            entityManager.getTransaction().commit();

            System.out.println("CheckPoint 4");

        } catch (Exception e) {
            entityManager.getTransaction().rollback();
            e.printStackTrace();

        } finally {
            entityManager.close();
        }
    }

    @Override
    public Optional<TableEntity> selectSequenceEntity(Long id) {
        return Optional.empty();
    }
}
```

####  IdGenerationApplication

```java
package studio.aroundhub.id_generation;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import studio.aroundhub.id_generation.factory.CEntityManagerFactory;
import studio.aroundhub.id_generation.service.IdGenerationService;
import studio.aroundhub.id_generation.service.impl.IdGenerationServiceImpl;

public class IdGenerationApplication {
    public static void main(String[] args) throws IOException {

        CEntityManagerFactory.initialization();
        IdGenerationService idGenerationService = new IdGenerationServiceImpl();

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        while (true) {
            System.out.println("Input your Command // [command] [name]");
            
            String commandLine = br.readLine();
            String[] splitCommand = commandLine.split(" ");

            // 별도 값 검증하는 로직은 추가하지 않음
            if (splitCommand[0].equalsIgnoreCase("exit")) {
                System.out.println("System closed");
                
                break;

            } else if (splitCommand[0].equalsIgnoreCase("insertDirect")) {
                idGenerationService.insertDirectEntity(splitCommand[1]);

            } else if (splitCommand[0].equalsIgnoreCase("insertIdentity")) {
                idGenerationService.insertIdentityEntity(splitCommand[1]);

            }else if (splitCommand[0].equalsIgnoreCase("insertTable")) {
                idGenerationService.insertTableEntity(splitCommand[1]);

            }else if (splitCommand[0].equalsIgnoreCase("insertSequence")) {
                idGenerationService.insertSequenceEntity(splitCommand[1]);
            }
        }
    }
}
```

### 실행 결과

```console
Hibernate: create sequence my_seq start with 1 increment by 1
2월 29, 2024 7:57:53 오후 org.hibernate.resource.transaction.backend.jdbc.internal.DdlTransactionIsolatorNonJtaImpl getIsolatedConnection
INFO: HHH10001501: Connection obtained from JdbcConnectionAccess [org.hibernate.engine.jdbc.env.internal.JdbcEnvironmentInitiator$ConnectionProviderJdbcConnectionAccess@5e5073ab] for (non-JTA) DDL execution was not in auto-commit mode; the Connection 'local transaction' will be committed and the Connection will be set into auto-commit mode.
Hibernate: 
    
    create table direct (
       number bigint not null,
        created_at datetime(6),
        name varchar(255),
        updated_at datetime(6),
        primary key (number)
    ) engine=InnoDB
Hibernate: 
    
    create table id_gen (
       entity varchar(255) not null,
        next_id bigint,
        primary key (entity)
    ) engine=InnoDB
Hibernate: 
    
    insert into id_gen(entity, next_id) values ('table_gen',0)
Hibernate: 
    
    create table identity (
       number bigint not null auto_increment,
        created_at datetime(6),
        name varchar(255),
        updated_at datetime(6),
        primary key (number)
    ) engine=InnoDB
Hibernate: 
    
    create table sequence (
       id bigint not null,
        created_at datetime(6),
        name varchar(255),
        updated_at datetime(6),
        primary key (id)
    ) engine=InnoDB
Hibernate: 
    
    create table table_gen (
       number bigint not null,
        created_at datetime(6),
        name varchar(255),
        updated_at datetime(6),
        primary key (number)
    ) engine=InnoDB
2월 29, 2024 7:57:54 오후 org.hibernate.engine.transaction.jta.platform.internal.JtaPlatformInitiator initiateService
INFO: HHH000490: Using JtaPlatform implementation: [org.hibernate.engine.transaction.jta.platform.internal.NoJtaPlatform]
Input your Command // [command] [name]
```

![02-id_gen-table](/assets/img/posts/study/jpa/primary-key-generation-strategy/02-id_gen-table.jpg)
*@TableGeneration(table = "id_gen")*

![03-my_seq-table](/assets/img/posts/study/jpa/primary-key-generation-strategy/03-my_seq-table.jpg)
*@SequenceGeneratorsequence(Name = "my_seq")*

![04-insert-insertdirect-jack](/assets/img/posts/study/jpa/primary-key-generation-strategy/04-insert-insertdirect-jack.jpg)
*insertDriect jack 입력*

커밋할 때 쿼리 실행

![05-insert-insertdirect-jack-result](/assets/img/posts/study/jpa/primary-key-generation-strategy/05-insert-insertdirect-jack-result.jpg)
*insertDirect jack 입력 결과*

![06-insert-insertdirect-james](/assets/img/posts/study/jpa/primary-key-generation-strategy/06-insert-insertdirect-james.jpg)
*insertDirect james 입력*

커밋할 때 쿼리 실행

![07-insert-insertdirect-james-result](/assets/img/posts/study/jpa/primary-key-generation-strategy/07-insert-insertdirect-james-result.jpg)
*insertDirect james 입력 결과*

![08-insert-insertidentity-jack](/assets/img/posts/study/jpa/primary-key-generation-strategy/08-insert-insertidentity-jack.jpg)
*insertIdentity jack 입력*

`persist()` 메서드가 호출될 때 쿼리 실행

![09-insert-insertidentity-jack-result.jpg](/assets/img/posts/study/jpa/primary-key-generation-strategy/09-insert-insertidentity-jack-result.jpg)
*insertIdentity jack 입력 결과*

![10-insert-insertidentity-james](/assets/img/posts/study/jpa/primary-key-generation-strategy/10-insert-insertidentity-james.jpg)
*insertIdentity james 입력*

`persist()` 메서드가 호출될 때 쿼리 실행

![11-insert-insertidentity-james-result](/assets/img/posts/study/jpa/primary-key-generation-strategy/11-insert-insertidentity-james-result.jpg)
*insertIdentity james 입력 결과*

![12-insert-inserttable-jack](/assets/img/posts/study/jpa/primary-key-generation-strategy/12-insert-inserttable-jack.jpg)
*insertTable jack 입력*

PK를 시퀀스 역할을 수행하는 테이블을 생성하여 할당하는 전략이기 때문에, `persist()` 메서드가 호출될 때 `@TableGenerator` 어노테이션의 `name` 속성에서 명시한 `id_gen`이라는 테이블에 대해 `SELECT` 문을 먼저 실행하여 PK 값을 가져오고 바꾼 뒤, 영속성 컨텍스트에 있는 영속 객체의 1차 캐시에 PK값을 할당한 후, `UPDATE` 문을 실행하여 변경된 PK 값을 DB에 저장한다.

`Entity` 객체는 커밋할 때 쿼리가 실행되면서 동기화된다.

![13-insert-inserttable-jack-result(1)](/assets/img/posts/study/jpa/primary-key-generation-strategy/13-insert-inserttable-jack-result(1).jpg)
*insertTable jack 입력 결과(1)*

![14-insert-inserttable-jack-result(2)](/assets/img/posts/study/jpa/primary-key-generation-strategy/14-insert-inserttable-jack-result(2).jpg)
*insertTable jack 입력 결과(2)*

![15-insert-insertsequence-jack](/assets/img/posts/study/jpa/primary-key-generation-strategy/15-insert-insertsequence-jack.jpg)
*insertSequence jack 입력*

PK를 DB의 시퀀스를 이용하여 할당하는 전략이기 때문에, `persist()` 메서드가 호출될 때 `@SequenceGenerator` 어노테이션의 `SequenceName` 속성에서 명시한 `my_seq`이라는 시퀀스에 대해 `SELECT` 문을 먼저 실행하여 PK 값을 가져온 뒤, 영속성 컨텍스트에 있는 영속 객체의 1차 캐시에 PK값을 할당한다.

`Entity` 객체는 커밋할 때 쿼리가 실행되면서 동기화된다.

![16-insert-insertsequence-jack-result(1)](/assets/img/posts/study/jpa/primary-key-generation-strategy/16-insert-insertsequence-jack-result(1).jpg)
*insertSequence jack 결과(1)*

![17-insert-insertsequence-jack-result(2).jpg](/assets/img/posts/study/jpa/primary-key-generation-strategy/17-insert-insertsequence-jack-result(2).jpg)
*insertsSquence jack 결과(2)*



## 참고 자료

- [어라운드 허브 스튜디오 - Around Hub Studio, "기본키 생성 전략, Direct, Identity, Sequence, Table (Id Generation) [ JPA (Java Persistence API) ]", 2021-12-28](https://www.youtube.com/watch?v=N9-LLRTDPGs&list=PLlTylS8uB2fAnOge-kDI0ZQxgj78bdTOO&index=5){: target="_blnak" }