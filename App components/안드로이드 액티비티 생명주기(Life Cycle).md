# [안드로이드 액티비티 생명주기(Life Cycle)](https://brunch.co.kr/@mystoryg/80)

## 안드로이드 액티비티 생명주기
> 생명주기를 쉽게 이해하려면 실제 화면에 표시 유무를 생각하면 된다. 먼저 최초로 앱을 실행하면 onCreate()가 호출되는데 이때 초기화 관련 작업을 하면 좋다. 다음으로 onStart()가 호출되는데 이 시점부터 사용자가 액티비티를 볼 수 있다. 그리고 액티비티가 실제 사용자와 상호작용이 가능한 포그라운드에 위치하면 onResume()이 호출된다. 이 상태를 액티비티가 실행 중인 것으로 본다.

> 반대로 액티비티가 실행 중인 상태에서 사용자와 상호작용이 불가능한 상태, 즉 포커스를 잃은 상태가 되면 onPause()가 호출된다. onStop()은 액티비티가 더 이상 보이지 않을 때 호출된다. 그리고 액티비티가 종료되거나 앱 프로세스 자체가 종료되면 onDestroy()가 호출된다.

<p align="center">
  <img src="//t1.daumcdn.net/thumb/R1280x0/?fname=http://t1.daumcdn.net/brunch/service/user/2Kn8/image/LMm0LctaUHwEAW5jmD1B9R2N64w.PNG">
</p>
