---
title: "[Design Pattern]빌더 패턴(Builder Pattern)"
categories: [Software Architecture, Design Patterns]
tags: [Software Architecture, Design Pattern, 디자인 패턴, Builder Pattern, 빌더 패턴]
---

# 빌더 패턴(Builder Pattern)

## 빌더 패턴(Builder Pattern)이란?

- 복잡한 객체의 생성 과정을 추상화하여 **객체를 단계적으로 생성**하는 디자인 패턴
- **객체의 생성과 표현을 분리**하여 복잡한 객체를 만들고, 클라이언트가 객체의 내부 표현과 독립적으로 생성할 수 있도록 하는 것이 목적
- 주로 생성자에 매우 많은 매개변수가 필요한 경우나 객체 생성의 단계가 여러 단계인 경우에 사용됨

## 빌더 패턴의 종류

### Classic 빌더 패턴

- 가장 일반적인 형태로, 객체 생성을 위한 빌더 클래스를 사용
- 빌더 클래스는 객체의 각 부분을 설정하는 메서드들을 제공하고, 마지막으로 `build()` 메서드를 호출하여 객체를 생성

#### 구성 요소

##### Product

```java
class Product {

    private String name;
    private int price;

    public Product() {}

    public Product(String name, int price) {
        this.name = name;
        this.price = price;
    }
    
    @Override
    public String toString() {
        return "Product[name=" + name + ", price=" + price + "]";
    }
}
```

##### Builder

```java
class Builder {

    private String name;
    private int price;

    public Builder name(String name) {
        this.name = name;
        return this;
    }

    public Builder price(int price) {
        this.price = price;
        return this;
    }

    public Product build() {
        // 구체적인 생성 로직
        return new Product(name, price);
    }
}
```

- Product 클래스와 동일하게 필드를 구성
- **객체 자신(`return this`)을 반환하는 setter를 생성**
	+ setter 호출 후 연속적으로 체이닝(Chaining)하여 호출할 수 있게 됨
	+ 일반적인 setter와 다르다는 걸 구분하기 위해 메서드 이름을 `setField()` 형식이 아닌 `field()` 형식으로 작명
- 이 클래스는 객체 생성을 위한 구체적인 로직을 구현

#### 객체 생성 예시

```java
public class test {

    public static void main(String[] args) {
        Product product = new Builder().name("컴퓨터").price(1000).build();

        System.out.println(product.toString()); // Prodcut[name=컴퓨터, price=1000]
    }
}
```

### Simple 빌더 패턴

- *Effective Java*에서 소개됨
- **빌더 클래스를 제품 클래스의 내부 정적 클래스(`static inner class`)로 정의**하는 방법
  + 빌더와 제품 클래스 간의 긴밀한 연관성을 유지하면서도, 빌더를 독립적으로 사용할 수 있게 해줌

#### 구성 요소

##### Product

```java
public class Product {

    private String name;
    private int price;

    private Product(Builder builder) {
        this.name = builder.name;
        this.price = builder.price;
    }

    public static class builder {

        private String name;
        private int price;

        public Builder name(String name) {
            this.name = name;
            return this;
        }

        public Builder price(int price) {
            this.price = price;
            return this;
        }

        public Product build() {
            return new Product(this);
        }
    }

    @Override
    public String toString() {
        return "Prodcut[name=" + name + ", price=" + price + "]";
    }
}
```

- 빌더 클래스를 통해서만 생성되도록 Product 클래스의 생성자를 `private`로 정의

#### 객체 생성 예시

```java
public class test {

    public static void main(String[] args) {
        Product product = new Product.Builder().name("TV").price(1000).build();
        // Product product = new Builder().name("TV").price(5000).build();

        System.out.println(product.toString()); // Prodcut[name=TV, price=5000]
    }
}
```

### Director 빌더 패턴

- *Gof의 디자인 패턴*에서 소개됨

## 참고자료

- [인파_, *빌더(Builder) 패턴 - 완벽 마스터하기*, Inpa Dev, 2023-03-16](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EB%B9%8C%EB%8D%94Builder-%ED%8C%A8%ED%84%B4-%EB%81%9D%ED%8C%90%EC%99%95-%EC%A0%95%EB%A6%AC){: target="_blank" }