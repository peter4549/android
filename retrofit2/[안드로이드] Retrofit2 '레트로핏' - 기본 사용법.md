# [[안드로이드] Retrofit2 '레트로핏' - 기본 사용법](https://jaejong.tistory.com/33)

## Retrofit 이란?
* REST API 통신을 위해 구현된 동일 Squareup 사의 OkHttp 라이브러리의 상위 구현체
  * Retrofit은 OkHttp를 네트워크 계층으로 활용하고 그 위에 구축됨
* AsyncTask 없이 Background Thread 실행, Callback을 통해 Main Thread에서 UI 업데이트

## Retrofit 장점 / 단점
* 빠른 성능  
Okhttp는 AsyncTask를 사용 (AsyncTask의 3~10배의 성능차이가 난다고 함)

* 간단한 구현 - 반복된 작업을 라이브러리에 넘겨서 처리  
HttpUrlConection의 Connection / Input & OutputStream / URL Encoding 생성 및 할당의 반복작업  
OkHttp의 쿼리스트링, Request / Response 반복 설정 작업

* 가독성  
Annotation 사용으로 코드의 가독성이 뛰어남, 직관적인 설계가 가능

* 동기/비동기 쉬운 구현  
동기 Synchronous - 동시에 일어난다는 의미로, 요청-응답이 하나의 트랜잭션에서 발생  
요청 후 응답까지 대기한다는 의미
비동기 Asynchronous - 동시에 일어나지 않는다는 뜻으로, 요청-응답은 별개의 트랜잭션
요청 후 응답이 도착하면 Callback으로 받아서 처리

## 3가지 구성요소
