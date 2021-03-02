# [Android Architecture Components 소개 (1)](https://medium.com/@maryangmin/android-architecture-components-%EC%86%8C%EA%B0%9C-1-8e04491be1f6)
# [Android Architecture Components 소개 (2)](https://medium.com/@maryangmin/android-architecture-components-%EC%86%8C%EA%B0%9C-2-d41a9272e692)

## 3.ViewModel
> MVVM에서 뷰모델은 뷰와 Repository를 이어주고 데이터를 보관하는 역할을 합니다. 하지만 싱글톤 등의 별도 처리를 하지 않는다면 뷰모델은 뷰 인스턴스 변수로 종속됩니다. 뷰에 종속되면 유지되어야 하는 작업 또는 데이터가 화면회전 등의 생명주기에 따라 소실될 수 있습니다.

> AAC의 뷰모델은 데이터를 쉽게 생명주기와 분리하여 관리할 수 있도록 돕습니다. AAC의 ViewModel을 상속받은 뷰모델은 ViewModelProviders로 Scope를 관리할 수 있습니다. 해당 Scope 내에서는 하나의 인스턴스만을 유지하여 작업이 중복되거나 데이터가 소실되지 않도록 합니다.

# [Android Architecture Components 소개 (3) (완)](https://medium.com/@maryangmin/android-architecture-components-%EC%86%8C%EA%B0%9C-3-52980a9e22af)
