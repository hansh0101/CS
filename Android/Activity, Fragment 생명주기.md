# Activity, Fragment 생명주기

<aside>
💡 Activity, Fragment의 생명주기를 알아보자.

</aside>

### Activity LifeCycle

Activity LifeCycle은 Activity가 시작되고 종료되는 시점까지의 상태를 Activity LifeCycle이라 한다.

Activity LifeCycle에는 `onCreate()` , `onStart()` , `onResume()` , `onPause()` , `onStop()` , `onDestroy()` , `onRestart()` 가 있다.

![Untitled](Activity,%20%209ac76/Untitled.png)

- `onCreate()`
    - 이 함수는 필수적으로 구현해야 한다.
    - 전체 LifeCycle 동안 ‘한 번’만 실행된다.
    - 이 메서드에는 XML, 멤버 변수 정의, 일부 UI 구성 등의 설정을 한다.
- `onStart()`
    - 활성상태에 들어가면 이 함수가 호출된다. (사용자에게 보여지기 직전)
    - 호출되면 포그라운드에 보내 상호작용을 할 수 있도록 준비한다.
    - 주로 UI를 관리하는 코드를 초기화한다. 이 메서드는 매우 빠르게 완료되고 바로 `onResume()` 을 호출한다.
- `onResume()`
    - 사용자한테 화면이 보여지고 상호작용하는 메서드이다.
    - 어떤 이벤트가 발생하여 앱에서 포커스가 떠날 때까지 이 상태에 머무른다.
    - 이 상태에서는 생명주기 구성요소가 포그라운드에서 사용자에게 보이는 동안 실행해야 하는 모든 기능을 활성화한다. (ex - 카메라 미리보기)
    - 방해가 되는 이벤트가 발생하면 일시중지 상태에 들어가고, 시스템이 `onPause()` 를 호출한다. (ex - 전화 수신, 사용자가 다른 화면으로 이동, 기기 화면 off)
- `onPause()`
    - 사용자가 화면을 떠날 때 시스템이 첫 번째로 이 메서드를 호출한다. (상태가 포그라운드에 있지 않게 되었다는 것을 나타냄)
    - 포그라운드에 있지 않을 때 실행할 필요가 없는 기능을 모두 정지할 수 있다. (ex - 카메라 미리보기 정지)
    - 시스템 리소스, 센서 핸들(GPS), 그리고 사용자가 필요로 하지 않을 때 배터리 수명에 영향을 미칠 수 있는 모든 리소스를 해제할 수도 있다. (멀티윈도우 모드나 화면분할도 고려하면,UI 관련 리소스와 작업을 완전히 해제하거나 조정할 때는 `onPause()` 보다 `onStop()` 을 사용하는 것이 좋다.)
    - 이 메서드는 아주 잠깐 실행되므로 저장 작업을 하기에는 시간이 부족할 수 있다. 사용자 데이터를 저장하거나, 네트워크를 호출하거나, 데이터베이스 트랜잭션을 실행해서는 안 된다. 이 메서드가 끝나기 전에 완료하지 못할 수 있다. 무거운 작업은 `onStop()` 에서 하고, 데이터 저장은 `ViewModel` , `onSaveInstanceState()` 를 참조하는 것이 좋다.
- `onStop()`
    - 포커스가 완전히 빠졌을 때 시스템이 이 메서드를 호출한다. (화면 전체가 가려졌을 때 또는 백그라운드로 이동했을 때)
    - 이 메서드에서는 앱이 사용자에게 보이지 않는 동안 앱이 필요하지 않는 리소스를 해제하거나 조정해야 한다. (ex - 애니메이션 중지, 정확한 위치 업데이트에서 대략적 위치 업데이트로 전환)
    - `onStop()` 에서 무거운 작업을 실행해야 한다고 `onPause()` 에서 잠깐 적었는데, 이 말은 예를 들어 정보를 DB에 저장해야 하는데 적절한 위치를 찾지 못한 경우 이 메서드에서 저장할 수 있다. (하지만, 이 메서드는 항상 호출되는 것은 아니며 메모리가 부족할 경우 호출이 안 될 수 있다.)
- `onDestroy()`
    - Activity가 소멸되기 전에 호출된다. 시스템은 다음 중 하나에 해당할 때 이 메서드를 호출한다.
        - Activity가 종료되는 경우 (Stack에서 제거되거나 finish()를 호출한 경우)
        - 구성 변경(ex - 기기 회전 또는 멀티 윈도우 모드)으로 인해 시스템이 일시적으로 Activity를 소멸시키는 경우
    - `onStop()` 에서 해제하지 않은 모든 리소스를 해제해야 한다.
    - 이 메서드가 호출되는 경우 시스템이 즉시 인스턴스를 생성한 다음, 새로운 구성에서 인스턴스에 관해 `onCreate()` 를 호출한다.
- `onRestart()`
    - `onStop()` 상태에 있던 화면이 다시 접근했을 때 호출되는 메서드
    

**Activity에서 Activity로 이동할 때 LifeCycle 순서**

1. Activity에서 문자가 왔을 경우(화면이 일부 가려진 경우)
    
    `onCreate() ~ onResume()` → 문자 수신 → `onPause()` → 문자 사라짐 → `onResume()`
    
2. Activity A에서 Activity B로 이동
    
    `A onCreate() ~ onResume()` → 화면 이동 클릭 → `A onPause()` → `B onCreate() ~ onResume()` → `A onStop() ~ onDestroy()` (상황에 따라)
    
3. Activity에서 백그라운드로 갔다가 다시 포그라운드로 복귀 시
    
    `onCreate() ~ onResume()` → 홈버튼(백그라운드) → `onPause()` → `onStop()` → 앱 복귀 → `onRestart()` → `onStart()` → `onResume()`
    

**Activity에서 구성 변경(ex - 화면 회전)될 때 LifeCycle 순서**

```
D/MainActivity: onCreate
D/MainActivity: onStart
D/MainActivity: onResume
D/MainActivity: onPause
D/MainActivity: onStop
D/MainActivity: onSaveInstanceState
D/MainActivity: onDestroy
D/MainActivity: onStart
D/MainActivity: onRestoreInstanceState
D/MainActivity: onResume
```

### Fragment LifeCycle

Fragment LifeCycle은 Fragment가 시작되고 종료될 때까지의 상태를 Fragment LifeCycle이라고 한다.

Fragment LifeCycle에는 `onAttach()` , `onCreate()` , `onCreateView()` , `onViewCreated()` , `onActivityCreated()` , `onStart()` , `onResume()` , `onPause()` , `onStop()` , `onDestroyView()` , `onDestroy()` , `onDetach()` 가 있다.

![Untitled](Activity,%20%209ac76/Untitled%201.png)

- onAttach(Activity)
    - Activity에서 Fragment가 호출될 때 최초 한 번 호출되는 메서드
- onCreate(Bundle)
    - Fragment가 생성될 때 호출되는 메서드
- onCreateView(LayoutInflater, ViewGroup, Bundle)
    - Fragment의 View를 생성하는 메서드
- onActivityCreated(Bundle)
    - Activity에서 onCreate()가 호출된 Fragment에서 호출되는 메서드
- onStart()
    - Fragment가 사용자에게 보여지기 직전 호출되는 메서드
- onResume()
    - Fragment가 사용자와 상호작용할 수 있는 상태일 때 호출되는 메서드
- onPause()
    - 화면이 일부 가려졌을 때 호출되는 메서드
- onStop()
    - Fragment가 화면에서 사라졌을 때 호출되는 메서드
- onDestroyView()
    - Fragment의 View가 사라질 때 호출되는 메서드
- onDestroy()
    - Fragment가 제거될 때 호출되는 메서드
- onDetach()
    - Fragment가 Activity와 연결이 종료될 때 호출되는 메서드

### FragmentManager

- Fragment를 추가, 삭제 또는 교체하고 백스택에 추가하는 등의 작업을 실행하는 클래스
- Fragment의 변경사항 집합을 Transaction이라고 한다.

### FragmentTransaction

- 각 트랜잭션은 수행하고자 하는 변경사항의 집합이다.
- 변경사항을 설정하려면 add(), remove(), replace()와 같은 메서드를 사용해야 한다.

![Untitled](Activity,%20%209ac76/Untitled%202.png)

![Untitled](Activity,%20%209ac76/Untitled%203.png)