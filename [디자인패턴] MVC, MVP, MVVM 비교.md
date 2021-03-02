# [[디자인패턴] MVC, MVP, MVVM 비교](https://beomy.tistory.com/43)
> 각각의 역할을 나눠 코드 관리를 하자!

## 1. MVC
MVC는 Model + View + Controller를 합친 용어이다.

### 1) 구조
<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F7IE8f%2FbtqBRvw9sFF%2FAGLRdsOLuvNZ9okmGOlkx1%2Fimg.png" width="400" height="299" alt="Sublime's custom image" />
</p>

* Model: 애플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분이다.
* View: 사용자에게 보이는 UI 부분이다.
* Controller: 사용자의 입력(Action)을 받고 처리하는 부분이다.

### 2) 동작
MVC 패턴의 동작 순서는 아래와 같다.
1. 사용자의 Action들은 Controller에 들어오게 된다.
2. Controller는 사용자의 Action을 확인하고, Model을 업데이트한다.
3. Controller는 Model을 나타내줄 View를 선택한다.
4. View는 Model을 이용하여 화면을 나타낸다.

**\* 참고 - MVC에서 View가 업데이트 되는 방법**
* View가 Model을 이용하여 직접 업데이트 하는 방법
* Model에서 View에게 Notify하여 업데이트 하는 방법
* View가 Polling으로 주기적으로 Model의 변경을 감지하여 업데이트 하는 방법

### 3) 특징
Controller는 여러 개의 View를 선택할 수 있는 1:n 구조이다.  
Controller는 View를 선택할 뿐 직접 업데이트하지 않는다. (View는 Controller를 알지 못한다.)

### 4) 장점
MVC 패턴의 장점은 널리 사용되고 있는 패턴이라는 점에 걸맞게 가장 단순하다. 단순하다 보니 보편적으로 많이 사용되는 디자인패턴이다.

### 5) 단점
MVC 패턴의 단점은 View와 Model 사이의 의존성이 높다는 것이다. View와 Model의 높은 의존성은 애플리케이션이 커질수록 복잡해지고 유지보수를 어렵게 만들 수 있다.

## 2. MVP
MVP는 Model + View + Presenter를 합친 용어이다. Model과 View는 MVC 패턴과 동일하고, Controller 대신 Presenter가 존재한다.

### 1) 구조
<p align="center">
  <img src="https://blog.kakaocdn.net/dn/clZlsT/btqBTLzeUCL/IDA8Ga6Yarndgr88g9Nkhk/img.png" srcset="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&amp;fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FclZlsT%2FbtqBTLzeUCL%2FIDA8Ga6Yarndgr88g9Nkhk%2Fimg.png" width="400" height="297" alt="MVP" filename="mvp.png" filemime="image/jpeg" style="width: 400px; height: 297px;" original="yes">
</p>  
* Model: 애플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분이다.
* View : 사용자에게 보이는 UI 부분입니다.
* Presenter : View에서 요청한 정보로 Model을 가공하여 View에 전달해 주는 부분이다. View와 Model을 붙여주는 역할을 한다.

### 2) 동작
MVP 패턴의 동작 순서는 아래와 같다.

1. 사용자의 Action들은 View를 통해 들어온다.
2. View는 데이터를 Presenter에 요청한다.
3. Presenter는 Model에게 데이터를 요청한다.
4. Model은 Presenter에서 요청받은 데이터를 응답한다.
5. Presenter는 View에게 데이터를 응답한다.
6. View는 Presenter가 응답한 데이터를 이용하여 화면을 나타낸다.

### 3) 특징
Presenter는 View와 Model의 인스턴스를 가지고 있어 둘을 연결하는 역할을 한다.
Presenter와 View는 1:1 관계이다.

### 4) 장점
MVP 패턴의 장점은 View와 Model의 의존성이 없다는 것이다. MVP 패턴은 MVC 패턴의 단점이었던 View와 Model의 의존성을 해결하였다. (Presenter를 통해서만 데이터를 전달받기 때문에.)

### 5) 단점
MVC 패턴의 단점인 View와 Model 사이의 의존성은 해결되었지만, View와 Presenter 사이의 의존성이 높은 가지게 되는 단점이 있다. 애플리케이션이 복잡해질수록 View와 Presenter 사이의 의존성이 강해지는 단점이 있다.

## 3. MVVM
MVVM은 Model + View + View Model를 합친 용어이다. Model과 View은 다른 패턴과 동일하다.

### 1) 구조
<p align="center">
  <img src="https://blog.kakaocdn.net/dn/CiXz0/btqBQ1iMiVT/staXr7UO95opKgXEU01EY0/img.png" srcset="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&amp;fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCiXz0%2FbtqBQ1iMiVT%2FstaXr7UO95opKgXEU01EY0%2Fimg.png" width="400" height="297" alt="MVVM" filename="mvvm.png" filemime="image/jpeg" style="width: 400px; height: 297px;" original="yes">
</p>

### 2) 동작
MVVM 패턴의 동작 순서는 아래와 같다.

1. 사용자의 Action들은 View를 통해 들어온다.
2. View에 Action이 들어오면, Command 패턴으로 View Model에 Action을 전달한다.
3. View Model은 Model에게 데이터를 요청한다.
4. Model은 View Model에게 요청받은 데이터를 응답한다.
5. View Model은 응답 받은 데이터를 가공하여 저장한다.
6. View는 View Model과 Data Binding하여 화면을 나타낸다.

### 3) 특징
MVVM 패턴은 Command 패턴과 Data Binding 두 가지 패턴을 사용하여 구현되었다.
Command 패턴과 Data Binding을 이용하여 View와 View Model 사이의 의존성을 없앴다.
View Model과 View는 1:n 관계이다.

### 4) 장점
MVVM 패턴은 View와 Model 사이의 의존성이 없다. 또한 Command 패턴과 Data Binding을 사용하여 View와 View Model 사이의 의존성 또한 없앤 디자인패턴이다. 각각의 부분은 독립적이기 때문에 모듈화하여 개발할 수 있다.

### 5) 단점
MVVM 패턴의 단점은 View Model의 설계가 쉽지 않다는 점이다.
