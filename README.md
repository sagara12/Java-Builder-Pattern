# Java-빌드 패턴
> 빌더패턴이란?
* 디자인패턴중 하나로, 생성과 표현의 분리(?)란다. 
  쉽게 말해 생성자에서 인자가 많을때 고려해볼수있는것이 빌더패턴

## 점층적 필드 패턴
클래스를 설계하다보면, 필수로 받야할 인자들이 있고 선택적으로 받야할 인자들이 있다.
이때 필수적으로 값이 있어야할 멤버변수를 위해 생성자에 매개변수를 넣는다.
또한 선택적 인자를 받기위해 추가적인 생성자를 만든다.

```java
public class PersonInfo{

  private String name;         //필수적으로 받야할 정보

  private int age;                //선택적으로 받아도 되는 정보

  private int phonNumber;   //선택적으로 받아도 되는 정보

  public PersonInfo(String name){ // 필수니까 강제하기 위해 생성자에 매개변수 추가

     this.name = name;

  }

  public PersonInfo(String name, int age){

    this.name = name;

    this.age = age;

  }

  public PersonInfo(String name, int age, int phonNumber){

    this.name = name;

    this.age = age;

    this.phonNumber = phonNumber;

  }
}
```

## 자바 빈즈 패턴
* 자바 빈즈패턴을 사용하면 생성자의 단점으로 꼽혔던 가독성이 어느정도 해결된다.
다만, 코드량이 늘어나는 단점이 존재하고, 가장 문제가 되는점은 객체일관성이 깨진다는 것이다.
객체일관성이 깨진다는것은, 한번 객체를 생성할때 그 객체가 변할 여지가 있다는 것이다.
코드를 보면 객체를 생성하고 그뒤에 값을 붙임.

```java
public class PersonInfo{

  private String name;         //필수적으로 받야할 정보

  private int age;                //선택적으로 받아도 되는 정보

  private int phonNumber;   //선택적으로 받아도 되는 정보

  public PersonInfo(){

  }

  public void setName(String name){

    this.name = name;

  }

  public void setAge(int age){

    this.age = age;

  }

  public void setPhonNumber(int phonNumber){

    this.phonNumber = phonNumber;

  }
}
```

```java
PersonInfo personInfo = new PersonInfo( ); // 선 객체 생성 후 값 부여

personInfo.setName("Mommoo");         

personInfo.setAge(12);                    

personInfo.setPhonNumber(119);
```

## 빌더 패턴 
* 둘의 단점을 모두 보완해서 나타난것이 바로 빌더패턴이다.
정보들은 자바빈즈패턴처럼 받되, 데이터 일관성을 위해 정보들을 다 받은 후에 객체를 생성한다.

> 빌더 패턴 장점
* 불필요한 생성자의 제거
* 데이터의 순서에 상관없이 객체생성 가능
* 명시적 선언으로 이해하기가 쉽고
* 각 인자가 어떤 의미인지 알기 쉽다.
* setter메서드가 없으므로 변경 불가능한 객체를 만들수있다.
* 한번에 객체를 생성하므로 객체일관성이 깨지지 않는다.
* build()함수가 null인지 체크해주므로 검증이 가능한다.
* 안그러면 set하지않은 객체에대해 get을 하게되는경우 nullPointerExcetpion발생 등등의 문제

```java
ublic class PersonInfo {

    private String name;         //필수적으로 받야할 정보

    private int age;             //선택적으로 받아도 되는 정보

    private int phonNumber;      //선택적으로 받아도 되는 정보

    private PersonInfo() {

    }

    public static class Builder {

        private String name;

        private int age;

        private int phonNumber;


        public Builder(String name) { // 필수변수는 생성자로 값을 넣는다.

            this.name = name;

        }

        // 멤버변수별 메소드 - 빌더클래스의 필드값을 set하고 빌더객체를 리턴한다.

        public Builder setAge(int age) {

            this.age = age;

            return this;

        }

        public Builder setPhonNumber(int phonNumber) {

            this.phonNumber = phonNumber;

            return this;

        }

	// 빌더메소드
        public PersonInfo build() {

            PersonInfo personInfo = new PersonInfo();

            personInfo.name = name;

            personInfo.age = age;

            personInfo.phonNumber = phonNumber;
            
            return personInfo;

        }
    }
```

```java
personInfo personinfo = new PersonInfo
    .Builder("SeungJin")    		// 필수값 입력 ( 빌더클래스 생성자로 빌더객체 생성)
    .setAge(25)  			// 값 set
    .setPhoneNumber(1234)
    .build() 				// build() 가 객체를 생성해 돌려준다.
```
## Lombok @Builder
* 위에서 만든 빌더클래스를 직접 만들지 않아도 롬복플러그인이 지원해주는 어노테이션 하나로 클래스를 생성할 수 있다.
클래스 또는 생성자 위에 @Builder어노테이션을 붙여주면 빌더패턴 코드가 빌드된다.
생성자 상단에 선언시 생성자에 포함된 필드만 빌더에 포함!

```java
@Builder
public class Person {
    private final String name;
    private final int age;
    private final int phone;
}
```
```java
Person person = Person.builder() // 빌더어노테이션으로 생성된 빌더클래스 생성자
    .name("seungjin")
    .age(25)
    .phone(1234)
    .build();
```

## 참조
https://esoongan.tistory.com/82
