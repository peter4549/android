# [[Android] 레트로핏(Retrofit) 이란? - (설명)](https://hijjang2.tistory.com/445)

## 1. Retrofit 이란?
> 1. Retrofit은 **TypeSafe한 HttpClient 라이브러리**입니다.   
**TypeSafe**하다는게 어떤 의미일까요? 예, 바로 네트워크로부터 전달된 데이터를  
우리 프로그램에서 **필요한 형태의 객체**로 받을 수 있다는 의미입니다.
> 2. 사실 Retrofit은 기본적으로 OKHttp에 의존하고 있습니다.

## 2. HttpClient Library를 왜 사용할까?
> Http통신을 가장 간단히 사용한다면, `HttpURLConnection`을 많이 사용해보셨을 것 입니다.  
`Java.net`에 내장되어 있기때문에, 별도의 라이브러리 없이 사용했던 기억이 납니다.

### Http 개발의 어려움
> 보통 Http를 개발한다면, 아래의 것들을 고려해야 합니다.

> * 연결
> * 캐싱
> * 실패한 요청의 재시도
> * 스레딩
> * 응답 분석
> * 오류 처리

### HttpURLConnection
> `HttpURLConnection`은 가장 원시적인 방법의 HttpClient입니다.

> 장점
> * java.net 에 포함된 클래스로 별도의 라이브러리 추가가 필요 없습니다.
> * 자신이 원하는 방식으로 커스텀하여 사용할 수 있습니다.(단점이기도 함)

> 단점
> * 자유도가 높은 대신, 직접 구현해야하는 것들이 많습니다.

## 3. Retrofit 사용법

### 3.2 interface에 HTTP API 기술
> retrofit은 Interface에 기술된 명세를 Http API로 전환해 줍니다. 따라서 우선, 우리가 요청할 API들에 대한 명세만을 Interface에 기술해두면 됩니다.

#### API 어노테이션
> 기본적으로 `GET, POST, DELETE, PUT`을 지원합니다.

### 3.3 HTTP API 인터페이스의 구현체 생성

#### BASE_URL
> 우가 요청할 서버의 기본 URL 입니다.
