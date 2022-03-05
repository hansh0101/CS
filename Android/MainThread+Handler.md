# MainThread + Handler

<aside>
💡 MainThread와 Handler에 대해 알아보자.

</aside>

### UI 처리를 위한 메인 스레드

애플리케이션은 성능을 위해 멀티 스레드를 많이 활용하지만, UI를 업데이트하는 것에는 단일 스레드 모델이 적용된다. 멀티 스레드로 UI를 업데이트하면 동일한 UI 자원을 사용할 때 교착 상태(deadlock), 경합 상태(race condition) 등 여러 문제가 발생할 수 있다. 따라서 UI 업데이트를 메인 스레드에서만 허용한다.

![Untitled](MainThread%20a59a2/Untitled.png)

![Untitled](MainThread%20a59a2/Untitled%201.png)

교착상태 ([https://ko.wikipedia.org/wiki/교착_상태](https://ko.wikipedia.org/wiki/%EA%B5%90%EC%B0%A9_%EC%83%81%ED%83%9C))

경쟁상태 ([https://ko.wikipedia.org/wiki/경쟁_상태](https://ko.wikipedia.org/wiki/%EA%B2%BD%EC%9F%81_%EC%83%81%ED%83%9C))

앱 프로세스가 시작되면서 메인 스레드가 생성된다. 컴포넌트의 생명주기 메서드와 그 안의 메서드 호출은 기본적으로 메인 스레드에서 실행된다. 메인 스레드는 UI를 변경할 수 있는 유일한 스레드이기 때문에 메인 스레드를 UI 스레드로 부르기도 한다. Service, Broadcast Receiver, Application은 사용자 인터페이스(UI)가 아니기 때문에, UI 스레드에서 실행된다고 하면 개념을 혼동하기 쉽다. UI를 변경하는 유일한 수단이라는 의미를 강조하기 위해서 UI 스레드라 부른다고 이해하면 편할 것 같다.

- Android Application에서 Main Thread
    
    안드로이드 프레임워크 내부 클래스인 `android.app.ActivityThread` 가 애플리케이션의 메인 클래스이고, `ActivityThread` 의 `main()` 메서드가 애플리케이션의 시작 지점이다. `ActivityThread` 는 어떤 것도 상속하지 않은 클래스다. Activity만 관련되어 있는 것도 아니고, 모든 컴포넌트들이 다 관련되어 있다.
    
    `AppCompatActivity` → `FragmentActivity` → `ComponentActivity` → `androidx.core.app.ComponentActivity` → `Activity` → `ActivityThread` 로 들어가면 볼 수 있다.
    
    ```java
    ActivityThread.java
    
    public static void main(string[] args) {
    	/*...*/
    	// 1. 
    	Looper.prepareMainLooper();
    	/*...*/
    	ActivityThread thread = new ActivityThread();
    	thread.attach(false, startSeq);
    	
    	if (sMainThreadHandler == null) {
    		sMainThreadHandler = thread.getHandler();
    	}
    	/*...*/
    	// 2.
    	Looper.loop();
    
    	throw new RuntimeException("Main Thread loop unexpectedly exited");
    }
    ```
    
    1. `Looper.prepareMainLooper();` → 메인 Looper를 준비한다.
    2. `Looper.loop();` → UI Message를 처리한다. `Looper.loop()` 메서드에서 무한 반복문이 있기 때문에 main() 메서드는 프로세스가 종료될 때까지 끝나지 않는다.
    

### Looper 클래스

- **스레드별로 Looper 생성**
    
    Looper는 `TLS(Thread Local Storage)` 에 저장되고 꺼내어진다. `ThreadLocal<Looper>`에 `set()` 메서드로 새로운 Looper를 추가하고, `get()` 메서드로 Looper를 가져올 때 스레드별로 다른 Looper가 반환된다. 그리고 `Looper.prepare()` 에서 **스레드별로 Looper를 생성**한다. 특히 메인 스레드의 메인 Looper는 ActivityThread의 main() 메서드에서 `Looper.prepareMainLooper()` 를 호출하여 생성한다. `Looper.getMainLooper()` 를 사용하면 어디서든 메인 Looper를 가져올 수 있다.
    
- **Looper별로 MessageQueue 소유**
    
    **Looper는 각각의 MessageQueue를 가진다**. 특히 메인 스레드에서는 이 MessageQueue를 통해서 UI 작업에서 경합 상태를 해결한다.
    

### Message와 MessageQueue

**MessageQueue는 Message를 담는 자료구조이다**. 

Message를 생성할 때는 오브젝트 풀에서 가져온다. 오브젝트 풀은 Message를 최대 50개까지 저장한다.

### Handler 클래스

**Handler는 Message를 MessageQueue에 넣는 기능과 MessageQueue에서 꺼내 처리하는 기능을 함께 제공**한다. 

```java
Handler()
Handler(Handler.Callback callback)
Handler(Looper looper)
Handler(Looper looper, Handler.Callback callback)
```

Handler에는 4가지 생성자가 있다. **Handler는 Looper(결국 MessageQueue)와 연결**되어 있다. 기본 생성자는 바로 생성자를 호출하는 스레드의 Looper를 사용하겠다는 의미이다. 메인 스레드에서 Handler 기본 생성자는 앱 프로세스가 시작할 때 ActivityThread에서 생성한 메인 Looper를 사용한다. Handler 기본 생성자는 UI 작업을 할 때 많이 사용된다. 

- **Handler 동작**
    
    위에서 언급했듯이 `Handler` 는 Message를 MessageQueue에 보내는 것과 Message를 처리하는 기능을 함께 제공한다. `post()` , `postAtTime()` , `postDelayed()` 메서드를 통해서 `Runnable` 객체도 전달되는데, `Runnable` 도 내부적으로 Message에 포함되는 값이다.
    
- **Handler 용도**
    
    Handler는 일반적으로 UI 갱신을 위해 사용된다.
    
    1. **백그라운드 스레드에서 UI 업데이트**: 백그라운드 스레드에서 네트워크나 DB 작업을 하는 도중에 UI를 업데이트해야 한다. `AsyncTask` 에서도 내부적으로 Handler를 이용해서 `onPostExecute()` 메서드를 실행해서 UI를 업데이트한다.
    2. **메인 스레드에서 다음 작업 예약**: UI 작업 중에 다음 UI 갱신 작업을 MessageQueue에 넣어 예약한다. 작업 예약이 필요한 경우가 있는데, 예를 들어 Activity의 onCreate() 메서드에서(소프트 키보드를 띄우는 것이나, ListView의 setSelection()) 작업을 할 경우 잘 동작하지 않는다. 이 때 Handler에 Message를 보내면 현재 작업이 끝난 이후의 다음 타이밍에 Message를 처리한다.
- **Handler의 타이밍 이슈**
    
    원하는 동작 시점과 실제 동작 시점에서 차이가 생기는데, 이런 타이밍 이슈는 메인 스레드와 Handler를 이해하고 나면 해결할 수 있다. Activity의 onCreate() 메서드에서 Handler의 `post()` 메서드를 실행하면 어느 시점에 실행될까? 실제 `post()` 메서드에 전달되는 `Runnable` 이 실행되는 시점은 언제일까? 메인 스레드에서는 한 번에 하나의 작업밖에 못 하고, 여러 작업이 서로 엉키지 않기 위해서 메인 Looper의 MessageQueue에서 Message를 하나씩 꺼내서 처리한다는 것을 염두에 두자.
    
    MessageQueue에서 Message를 하나 꺼내오면 `onCreate() ~ onResume()` 까지 쭉 실행이 된다. 즉, **onCreate()에서 Handler의 post()에 전달한 Runnable은 onResume() 이후에 실행**된다.
    
- **지연 Message는 처리 시점을 보장할 수 없다.**
    
    Handler에서 전달된 지연 Message는 지연 시간을 정확하게 보장하지 않는다. MessageQueue에서 먼저 꺼낸 Message 처리가 오래 걸린다면 실행은 당연히 늦어진다.
    
    예를 들어 0.2초 후에 로그를 남기는 Handler가 있고, 0.5초를 멈추는 Handler가 있다고 가정하면 로그를 남기는 Handler는 0.2초 후가 아닌 0.5초 후에 로그를 남긴다.