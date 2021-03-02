# [[디자인패턴] MVC, MVP, MVVM 비교](https://beomy.tistory.com/43)
> 각각의 역할을 나눠 코드 관리를 하자!

## 1. MVC
MVC 패턴은 Model + View + Controller를 합친 용어입니다.

### 1) 구조
<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F7IE8f%2FbtqBRvw9sFF%2FAGLRdsOLuvNZ9okmGOlkx1%2Fimg.png" width="400" height="299" alt="Sublime's custom image" />
</p>

* Model: 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분입니다.
* View: 사용자에서 보여지는 UI 부분입니다.
* Controller: 사용자의 입력(Action)을 받고 처리하는 부분입니다.

### 2) 동작
MVC 패턴의 동작 순서는 아래와 같습니다.
1. 사용자의 Action들은 Controller에 들어오게 됩니다.
2. Controller는 사용자의 Action을 확인하고, Model을 업데이트합니다.
3. Controller는 Model을 나타내줄 View를 선택합니다.
4. View는 Model을 이용하여 화면을 나타냅니다.

**\* 참고 - MVC에서 View가 업데이트 되는 방법**
* View가 Model을 이용하여 직접 업데이트 하는 방법
* Model에서 View에게 Notify하여 업데이트 하는 방법
* View가 Polling으로 주기적으로 Model의 변경을 감지하여 업데이트 하는 방법.

### 3) 특징
Controller는 여러 개의 View를 선택할 수 있는 1:n 구조입니다.  
Controller는 View를 선택할 뿐 직접 업데이트하지 않습니다. (View는 Controller를 알지 못합니다.)

### 4) 장점
MVC 패턴의 장점은 널리 사용되고 있는 패턴이라는 점에 걸맞게 가장 단순합니다. 단순하다 보니 보편적으로 많이 사용되는 디자인패턴입니다.

### 5) 단점
MVC 패턴의 단점은 View와 Model 사이의 의존성이 높다는 것입니다. View와 Model의 높은 의존성은 어플리케이션이 커질수록 복잡하지고 유지보수가 어렵게 만들 수 있습니다.

## 2. MVP
MVP 패턴은 Model + View + Presenter를 합친 용어입니다. Model과 View는 MVC 패턴과 동일하고, Controller 대신 Presenter가 존재합니다.

### 1) 구조
<p>
  <img src="https://blog.kakaocdn.net/dn/clZlsT/btqBTLzeUCL/IDA8Ga6Yarndgr88g9Nkhk/img.png" srcset="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&amp;fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FclZlsT%2FbtqBTLzeUCL%2FIDA8Ga6Yarndgr88g9Nkhk%2Fimg.png" width="400" height="297" alt="MVP" filename="mvp.png" filemime="image/jpeg" style="width: 400px; height: 297px;" original="yes">
</p>
