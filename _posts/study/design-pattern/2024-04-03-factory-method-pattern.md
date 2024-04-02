---
title: "[Design Pattern]팩토리 메서드 패턴(Factory Method Pattern)"
categories: [Study, Design Pattern]
tags: [Design Pattern, 디자인 패턴, Factory Method Pattern, 팩토리 메서드 패턴]
---

# 팩토리 메서드 패턴(Factory Method Pattern)

## 팩토리 메서드 패턴(Factory Method Pattern)이란?

- 객체 생성을 처리하기 위한 소프트웨어 디자인 패턴
- 객체 생성을 하위 클래스에 위임하여 객체 생성 방법을 캡슐화
- 클라이언트 코드는 객체 생성에 필요한 구체적인 클래스를 알 필요 없이 추상적인 인터페이스를 통해 객체를 생성할 수 있음

## 팩토리 메서드 패턴의 주요 구성 요소

- Creator(생성자)
	+ 객체 생성을 위한 인터페이스를 정의
	+ 이 인터페이스에는 팩토리 메서드가 포함되어 있음
	+ 이 메서드는 객체 생성을 위한 추상 메서드로, 하위 클래스에 구체적인 객체 생성 방법을 위임
- Concrete Creator(구체적 생성자)
	+ Creator 클래스를 상속받아 구체적인 객체 생성 방법을 구현
	+ 팩토리 메서드를 오버라이드하여 필요한 구체적인 객체를 생성하고 반환
- Product(제품)
	+ 팩토리 메서드에 의해 생성되는 객체에 대한 인터페이스를 정의
	+ Creator 클래스는 이 인터페이스를 통해 생성된 객체를 사용
- Concrete Product(구체적 제품)
	+ Product 인터페이스를 구현하여 실제로 생성되는 객체를 나타냄
	+ 각 Concrete Creator는 이러한 Concrete Product 중 하나를 생성

## 예시

```java
// Product: 도서를 나타내는 인터페이스
interface Book {
    void display();
}

// Concrete Products: 각각의 구체적인 도서 클래스
class FictionBook implements Book {
    @Override
    public void display() {
        System.out.println("Fiction Book");
    }
}

// Concrete Products: 각각의 구체적인 도서 클래스
class NonFictionBook implements Book {
    @Override
    public void display() {
        System.out.println("Non-Fiction Book");
    }
}

// Creator: 도서 생성을 위한 추상 클래스
abstract class BookFactory {
    abstract Book createBook();
}

// Concrete Creators: 각각의 구체적인 도서 생성 클래스
class FictionBookFactory extends BookFactory {
    @Override
    Book createBook() {
        return new FictionBook();
    }
}

// Concrete Creators: 각각의 구체적인 도서 생성 클래스
class NonFictionBookFactory extends BookFactory {
    @Override
    Book createBook() {
        return new NonFictionBook();
    }
}

// Client: 도서 생성 및 사용
public class Main {

    public static void main(String[] args) {

        // Fiction Book을 생성하는 Factory를 사용하는 예시
        BookFactory fictionFactory = new FictionBookFactory();

        Client client1 = new Client(fictionFactory);
        client1.displayBook();  // 출력: Fiction Book

        // Non-Fiction Book을 생성하는 Factory를 사용하는 예시
        BookFactory nonFictionFactory = new NonFictionBookFactory();

        Client client2 = new Client(nonFictionFactory);
        client2.displayBook();  // 출력: Non-Fiction Book
    }
}

class Client {

    private BookFactory factory;

    public Client(BookFactory factory) {
        this.factory = factory;
    }

    public void displayBook() {
        Book book = factory.createBook();
        book.display();
    }
}
```