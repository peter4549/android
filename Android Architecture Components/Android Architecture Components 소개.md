# [Android Architecture Components 소개 (1)](https://medium.com/@maryangmin/android-architecture-components-%EC%86%8C%EA%B0%9C-1-8e04491be1f6)
# [Android Architecture Components 소개 (2)](https://medium.com/@maryangmin/android-architecture-components-%EC%86%8C%EA%B0%9C-2-d41a9272e692)

## 3.ViewModel
> MVVM에서 뷰모델은 뷰와 Repository를 이어주고 데이터를 보관하는 역할을 합니다. 하지만 싱글톤 등의 별도 처리를 하지 않는다면 뷰모델은 뷰 인스턴스 변수로 종속됩니다. 뷰에 종속되면 유지되어야 하는 작업 또는 데이터가 화면회전 등의 생명주기에 따라 소실될 수 있습니다.

```
class MainActivity extends LifecycleActivity {
    private LiveData<User> userData;
    private MyViewModel viewModel;
    public void onCreate(Bundle savedInstanceState) {
        String userId = "userId@gmail.com";
        // fetchUser()가 종료되지 않았을 때 화면회전이 일어나면 onCreate()가 다시 불리면서 fetchUser()가 중복하여 일어난다.
        userData = viewModel.fetchUser(userId);
        userData.observe(this, user -> {
            // update UI
        });
    }
}
```

> AAC의 뷰모델은 데이터를 쉽게 생명주기와 분리하여 관리할 수 있도록 돕습니다. AAC의 ViewModel을 상속받은 뷰모델은 ViewModelProviders로 Scope를 관리할 수 있습니다. 해당 Scope 내에서는 하나의 인스턴스만을 유지하여 작업이 중복되거나 데이터가 소실되지 않도록 합니다.

```
// AAC의 ViewModel을 상속받는다.
class MyViewModel extends ViewModel {
    private LiveData<User> userData;
    public LiveData<User> getUser(String userId) {
        if (userData == null) userData = webservice.fetchUser(userId);
        return userData;
    }
}

class MainActivity extends LifecycleActivity {
    public void onCreate(Bundle savedInstanceState) {
        String userId = "userId@gmail.com";
        // 처음이면 this Scope에 종속된 MyViewModel를 생성한다.
        // this Scope에 종속된 MyViewModel가 이미 있다면 불러온다.
        ViewModelProviders.of(this)
                .get(MyViewModel.class)
                // 화면회전이 일어나 다시 호출되어도 같은 인스턴스 이므로 중복작업이 일어나지 않는다.
                .getUser(userId)
                .observe(this, user -> {
                    // update UI
                });
    }
}
```

> 위 예시코드에서는 this를 이용해 뷰모델의 Scope를 Activity로 지정하였습니다. Activity를 Scope로 하면 속한 Fragment 간에도 하나의 뷰모델을 공유하여 데이터를 전달할 수 있습니다.

# [Android Architecture Components 소개 (3) (완)](https://medium.com/@maryangmin/android-architecture-components-%EC%86%8C%EA%B0%9C-3-52980a9e22af)
