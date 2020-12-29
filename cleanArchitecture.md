# Clean Architecture Reading
---

##1장

#### *설계와 아키텍처란?*
>설계와 아키텍처 : 차이가 존재하지 않는다.
저 수준의 세부사항과 고 수준의 세부사항 모두 소프트웨어 설계의 구성요소로 포함된다. 
결과적으로 이런 설계와 아키텍처는 *시스템을 만들고 유지보수 하는데 들어가는 **인풋을 최소화하려는 목적**으로 동일 하다.*

>초기 아키텍처가 제대로 잡히지 않은 상태로 개발된 소프트웨어는 점점 낮아지는 생산성의 문제에 직면할 수 밖에 없다.

#### **두가지 가치** 
1. 행위 : 이해관계자가 원하는 소프트웨어를 만드는것
2.  아키텍처 : 이해관계자가 원하는 요구사항에 맞춰 시스템을 구축할 수 있도록 해주는 것
> 소프트웨어를 개발하는 행위를 넘어서 이해관계자의 요구사항, 요구사항의 변화에 맞춰 시스템을 구축할 수 있도록, 아키텍처는 형태에 독립적이어야 한다.



##2장

#### *패러다임*
>
>1. 구조적 프로그래밍
    제어프름의 직접적인 전환에 대해 규칙을 부과한다.
>2. 객체지향 프로그래밍
    제어흐름의 간접적인 전환에 대해 규칙을 부과한다.
>3. 함수형 프로그래밍
    할당문에 규칙을 부과한다.

#### 구조적 프로그래밍
>goto문을 통한 프로그램의 제어흐름을 개발자가 직접 전환할 수 있는 구조의 문제를 해결하기위해 테이크스트라는 순차, 분기, 반복의 세가지 구조만으로도 프로그램의 제어가 가능하다는 사실을 증명해냈다. 
결과적으로 현재의 우리가 다루는 개발언어들은 개발자가 임의적으로 프로그램의 제어흐름을 변경할 수 있는 구문을 제공하지 않는다. (JAVA의 brak문은 예외적인 경우.)
>
>goto문을 통한 임의적 제어흐름을 막고 순차, 분기, 반복의 구조로 프로그램을 개발하도록 만든 패러다임은 프로그램의 오류를 검출할 수 있는 반증의 단위(버그검출)를 만들어 주었다. 마치 과학이 수 많은 검증을 통해 밝혀진 부분까진 참이라 가정하고 이후의 논리를 build up 해 나가는 것과 같이.


#### 객체지향 프로그래밍
>객체지향 프로그래밍의 언어는 적어도 다음 세가지의 요건을 갖춰야 한다.
    1. 캡슐화
        하나의 캡슐 단위에 함수와 데이터를 응집력있게 구성하여 구분지을 수 있도록 해준다. 
    2. 상속 
    4. 다형성
    다형성을이용하 시스템의 모든 소스코드 의존성에 대한 절대적 제어권한을 획득할 수 있다. 

#### 함수형 프로그래밍
``` js
    //1
    let i = 0;
    for(i < 25; i++){
        console.log(i);
    }
    //2
    Array.from({length:25},(undefiend,i)=>{console.log(i);});

    이 형식이 함수형 프로그래밍 설명에 적합한지 잘 모르겠음;;
```
위 의 두가지 코드 형식이 있다고 하였을때, 1번의 경우 선언한 변수 i를 이용하여 console.log();를 실행한다.반면 2번의 경우 fn()에 start와 end를 입력하여 console.log();를 수행하도록 만들었다.



1번과 같은 선언된 변수는 동시성 어플리케이션에서 다수의 스레드와 프로세스 환경에서 경합조건, 교착상태 조건, 동시업데이트 등과 같은 문제를 발생시킬 수 있다. 반면 2번과 같은 형식은 프로그램은 이러한 가변 변수로 인한 문제의 원인을 해소할 수 있다.



## 3장
>좋은 아키텍처를 구현하기 위해 필수적인 원칙5가지(SOLID)
    1. 단일책임의 원칙 (Single Responsibility Principle) SAP
    2. 개방-폐쇄의 원칙 (Open-Closed Principle)  OCP
    3. 리스코프 치환 원칙 (Liskov Substitution Principle) LSP
    4. 인터페이스 분리 원칙 (Interface Segregation Principle) ISP
    5. 의존성 역전 원칙 (Dependency Inversion Principle) DIP



#### 단일책임의 원칙 (Single Responsibility Principle) SAP
>하나의 모듈(함수와 데이터의 응집단위)은 하나의 액터(이해관계자/집단)에 대해서만 책임져야 한다.

```java

/*===========================
============1================
=============================*/
class Employee{
    //회계팀이 사용하는 method
    public void calcPay(){
        /*logic*/
    }
    //인사팀이 사용하는 method
    public void reportHours(){
        /*logic*/
    }
    //DBA가 사용하는 method
    public void save(){
        /*logic*/
    }
}

/*===========================
============2================
=============================*/
class Caculator{
    public void calcPay(){
        /*logic*/
    }
}
class Reporter{
    public void reportHours(){
        /*logic*/
    }
}
class Saver{
    public void save(){
        /*logic*/
    }   
}


/*================================
============3 퍼사드패턴===========
==================================*/
class EmployeeFacade{
    public Caculator caculator;
    public Reporter reporter;
    public Saver saver;

    public void calcPay(){
        this.caculator.calcPay();
    }
    public void reportHours(){
        this.reporter.reportHours();
    }
    public void save(){
        this.saver.save();
    }
}

class Caculator{
    public void calcPay(){
        /*logic*/
    }
}
class Reporter{
    public void reportHours(){
        /*logic*/
    }
}
class Saver{
    public void save(){
        /*logic*/
    }
}

```

>위 코드에서 1번의 경우
한 class에서 필요로 하는 method가 서로 다른 여러 이해관계자(인사팀, DBA, 회계팀)이 함께 소스를 CUD하면서 일어나는 병합, 중복의 문제가 발생할 수 있다. 이러한 문제를 해결하기위해 2번처럼 class를 나눌 수 있으나 이런 경우 개발자가 각각의 인스턴스를 매번 추적해야하는 불편함이 있다.
따라서 3번(퍼사드 패턴)과 같은 방식으로 해결을 할 수 있다.
이러한 해결법의 궁극적 목적은 **단일책임원칙의 충족**에 있다. 1번 소스에서 발생했던 병합, 중복의 문제를 피하기 위함이다.


#### 개방-폐쇄의 원칙 (Open-Closed Principle)  OCP
>소프트웨어 개체는 확장에는 열려있어야 하고, 변경에는 닫혀있어야 한다.
#### 리스코프 치환 원칙 (Liskov Substitution Principle) LSP

```java
    class T{}
    class S /*extends T 이게 생략된게 아닐까*/{}
    S o1;
    T o2;
```
>위와 같은 코드가 있을때, T 타입을 포함하여 만든 어떤 프로그램에서 T타입으로 만들어진 모든 인스턴스를 o1으로 대체하여도 프로그램에 아무런 문제도 주지않는다면, S 는 T의 하위타입이라는 사실이 성립한다.

이같은 하위타입이 상위타입을 치환하도록 하여 구성하도록 강제하는것은 시스템이 비대해 질수록 유용하다.
만약 A -> B -> C -> D,E 와 같은 크기로 구성된 시스템이 있다했을때 D는 C를, C는 B를, B는 A를 각각 치환하도록 구현이 된 반면 E는 이 위의 타입들을 포함하는 개념이지만 이들을 실제로는 치환하지 않도록 구성해 놓았다면
우리가 코드에서 E를 사용할 때 마다 E가 사실은 A ~ C를 치환하는 개념임을 구현하거나 validation check를 해주어야 하는 상황이 생길 수 있다. 따라서 비대한 코드를 작성할 시 하위개념으로 진행되는동안 상위개념을 치환하도록 하는것은 효율적인 방법이 될 수 있다.


#### 인터페이스 분리 원칙 (Interface Segregation Principle) ISP  


#### 의존성 역전 원칙 (Dependency Inversion Principle) DIP