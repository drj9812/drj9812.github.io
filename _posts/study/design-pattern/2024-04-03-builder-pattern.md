---
title: "[Design Pattern]빌더 패턴(Builder Pattern)"
categories: [Study, Design Pattern]
tags: [Design Pattern, 디자인 패턴, Builder Pattern, 빌더 패턴]
---

# 빌더 패턴(Builder Pattern)

## 빌더 패턴(Builder Pattern)이란?

- 복잡한 객체의 생성 과정을 추상화하여 객체를 단계적으로 생성하는 디자인 패턴
- 객체의 생성과 표현을 분리하여 복잡한 객체를 만들고, 클라이언트가 객체의 내부 표현과 독립적으로 생성할 수 있도록 하는 것이 목적
- 주로 생성자에 매우 많은 매개변수가 필요한 경우나 객체 생성의 단계가 여러 단계인 경우에 사용됨

## 빌더 패턴의 구성 요소

- Product(제품)
	+ 복잡한 객체를 나타냄
	+ 이 객체는 생성되는 과정에서 Builder에 의해 구축됨
- Builder(건축자)
	+ 제품을 생성하기 위한 인터페이스를 정의
	+ 이 인터페이스에는 객체의 각 부분을 생성하기 위한 메서드가 포함됨
- Concrete Builder(구체적 건축자)
	+ Builder 인터페이스를 구현하여 제품을 실제로 구축하는 클래스
	+ 이 클래스는 객체 생성을 위한 구체적인 로직을 구현
- Director(감독자)
	+ 제품의 생성 과정을 조정하는 클래스
	+ 이 클래스는 Builder 인터페이스를 사용하여 제품을 구축하는 메서드를 정의함

## 예시

```java
// Product: 복잡한 객체를 나타내는 클래스
class Computer {

    private String cpu;
    private String gpu;
    private int ram;
    private int storage;

    public Computer(String cpu, String gpu, int ram, int storage) {
        this.cpu = cpu;
        this.gpu = gpu;
        this.ram = ram;
        this.storage = storage;
    }

    // 각 필드의 Getter 메서드들
    // 이 예시에서는 Setter 메서드를 생략했지만, 실제 프로젝트에서는 필요할 수 있다.
    // ...

    @Override
    public String toString() {
        return "Computer(" +
                "cpu='" + cpu + '\'' +
                ", gpu='" + gpu + '\'' +
                ", ram=" + ram +
                ", storage=" + storage +
                ')';
    }
}

// Builder: Computer 객체를 생성하는 인터페이스
interface ComputerBuilder {

    Computer build();
}

// Concrete Builder: ComputerBuilder 인터페이스를 구현하여 Computer 객체를 생성하는 클래스
class ComputerBuilderImpl implements ComputerBuilder {

    private String cpu;
    private String gpu;
    private int ram;
    private int storage;

    public ComputerBuilderImpl() {
        // 기본값 설정 등 초기화 작업 수행
    }

    public ComputerBuilderImpl setCpu(String cpu) {
        this.cpu = cpu;
        return this;
    }

    public ComputerBuilderImpl setGpu(String gpu) {
        this.gpu = gpu;
        return this;
    }

    public ComputerBuilderImpl setRam(int ram) {
        this.ram = ram;
        return this;
    }

    public ComputerBuilderImpl setStorage(int storage) {
        this.storage = storage;
        return this;
    }

    @Override
    public Computer build() {
        return new Computer(cpu, gpu, ram, storage);
    }
}

// Director: 객체 생성 과정을 조정하는 클래스
class ComputerDirector {

    private ComputerBuilder builder;

    public ComputerDirector(ComputerBuilder builder) {
        this.builder = builder;
    }

    public Computer buildComputer() {
        return builder
                .setCpu("Intel Core i7")
                .setGpu("NVIDIA GeForce RTX 3080")
                .setRam(16)
                .setStorage(512)
                .build();
    }
}

// Client: Builder 패턴을 사용하여 Computer 객체를 생성하는 예시
public class Main {

    public static void main(String[] args) {

        ComputerBuilder builder = new ComputerBuilderImpl();
        ComputerDirector director = new ComputerDirector(builder);

        Computer computer = director.buildComputer();
        System.out.println(computer); // Computer{cpu='Intel Core i7', gpu='NVIDIA GeForce RTX 3080', ram=16, storage=512}
    }
}
```

```java
public class Book {

    private String title;
    private String author;
    private String publisher;
	
    public Book(String title, String author, String publisher) {
        this.title = title;
	this.author = author;
	this.publisher = publisher;
    }
	
    public String toString() {
        return String.format("Book(title=%s, author=%s, publisher=%s)", this.title, this.author, this.publisher);
    }
	
    public static BookBuilder builder() {
        return new BookBuilder();
	// 외부 클래스는 static으로 선언된 내부 클래스의 private 생성자를 호출할 수 있음
    }
	
    public static class BookBuilder {
        private String title;
        private String author;
	private String publisher;
		
        private BookBuilder() {}
		
        public BookBuilder title(String title) {
            this.title = title;
            return this; // this : BookBuilder 타입으로 생성된 객체(인스턴스)
	}
		
	public BookBuilder author(String author) {
            this.author = author;
            return this;
	}
		
	public BookBuilder publisher(String publisher) {
            this.publisher = publisher;
            return this;
	}
		
	public Book build() {
	    return new Book(title, author, publisher);
	}
    }
	
    public static void main(String[] args) {

        // Book 타입의 객체 생성
        Book book1 = new Book("하얼빈", "김훈", "문학동네");
        System.out.println(book1); // Book(title=하얼빈, author=김훈, publisher=문학동네)
		
        Book book2 = new Book("홍길동", "허균", "조선");
        System.out.println(book2); // Book(title=홍길동, author=허균, publisher=조선)
		
        Book book3 = new Book("허균", "홍길동전", "모름");
        //생성자의 파라미터 이름을 모호하게 선언한다면 호출하는 입장에서 적절한 값을 입력할 수가 없음
	System.out.println(book3); // Book(title=허균, author=홍길동전, publisher=모름)
		
	Book book4 = Book.builder().author("허균").title("홍길동전").build(); // 파라미터 순서 상관 없음
	System.out.println(book4); // Book(title=홍길동전, author=허균, publisher=null)
		
	Book book5 = Book.builder().author("남궁성").title("자바의 정석").build();
	System.out.println(book5); // Book(title=자바의 정석, author=남궁성, publisher=null)
    }
}
```