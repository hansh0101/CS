# Intent

<aside>
💡 Intent에 대해 알아보자.

</aside>

인텐트는 messaging object(메시지 객체)다. 이 객체를 통해 다른 컴포넌트 간에 정보를 주고받을 수 있다.

### Intent Types

1. **`Explicit Intents`**
    
    명시적 인텐트는 인텐트에 클래스 객체나 컴포넌트 이름을 지정하여 호출할 대상을 확실히 알 수 있는 경우에 사용한다. (주로 애플리케이션 내부에서 사용)
    
2. **`Implicit Intents`**
    
    암시적 인텐트는 호출할 대상이 달라질 수 있는 경우에 사용한다. (안드로이드 시스템이 인텐트를 이용해 요청한 정보를 처리할 수 있는 적절한 컴포넌트를 찾아 사용자에게 그 대상과 처리 결과를 보여준다.)
    

![Untitled](Intent%206ad32/Untitled.png)

### IntentFilter

암시적 인텐트를 통해 사용자로 하여금 어느 앱을 사용할지 선택하도록 할 때 IntentFilter가 필요하다.

### PendingIntent

Intent를 가지고 있는 클래스로, 기본 목적은 다른 애플리케이션(다른 프로세스)의 권한을 허가하여 가지고 있는 Intent를 마치 본인 앱의 프로세스에서 실행하는 것처럼 사용하는 것이다.

Notification은 안드로이드 시스템의 NotificationManager가 Intent를 실행한다. 즉 다른 프로세스에서 수행하기 때문에 Notification으로 Intent 수행 시 PendingIntent의 사용이 필수적이다.