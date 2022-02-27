# ANR

<aside>
💡 ANR에 대해 알아보자.

</aside>

> Application Not Responding
> 

Android 앱의 UI 스레드가 너무 오랫동안 차단되면 `ANR(애플리케이션 응답 없음)` 오류가 나타나게 된다. 앱이 포그라운드에 있으면 아래와 같이 시스템에서 사용자에게 다이얼로그를 표시한다.

![Untitled](ANR%20a9f7a/Untitled.png)

Android 프레임워크에서 ANR 관련한 내용은 `com.android.server.am.ActivityManagerService` 에서 확인할 수 있다. (ActivityManagerService는 system_server 프로세스에서 실행된다.)

```java
static final int BROADCAST_FG_TIMEOUT = 10 * 1000;
static final int BROADCAST_BG_TIMEOUT = 60 * 1000;
static final int KEY_DISPATCHING_TIMEOUT = 5 * 1000;
```

### ANR이 발생하는 경우

- 화면 터치와 키 입력에서 ANR
    - 메인 스레드를 어디선가 이미 점유하고 있다면 키 이벤트를 전달하지 못하는데, 이벤트를 전달할 수 없는 시간이 타임아웃을 넘는다면 이 때 ANR이 발생한다. **키 이벤트인 볼륨, 메뉴, 백 키의 경우는 눌리고서 5초 이상 지연 시 바로 ANR을 발생**시킨다. 참고로 홈 키와 전원 키는 앱과 별개로 동작하고 ANR 발생과는 무관하다.
    - **터치 이벤트는 경우가 다르다**. 터치 이벤트도 메인 스레드가 사용 중이라면 대기하는 것은 동일하지만 타임아웃된다고 해서 바로 ANR이 발생하지 않는다. 그 다음으로 이어서 터치 이벤트가 왔을 때는 **두 번째 터치 이벤트가 전달되지 않는 시간이 타임아웃되면 ANR이 발생**한다. 예를 들어, 어디선가 메인 스레드를 블로킹하고 있는데 이 때 첫 번째 터치 이벤트만으로는 ANR이 발생하지 않는다. 두 번째 터치 이벤트가 있고서 5초가 지나면 그때서야 ANR이 발생한다.
- Message 처리 각각이 5초 이내라도 총합 처리 시간 영향
    - 가끔 혼동하는 경우가 있는데, 특정 Message 처리가 5초가 넘더라도 그 사이에 터치가 없을 때는 문제가 발생하지 않는다. 예를 들어, for문을 0부터 4까지 돌리는데 2초씩 메인 스레드를 블로킹하고 있다고 가정하자. 5개의 Message를 처리하는 시간은 총 10초다. 가만히 두면 문제가 없다. 하지만 **Message를 처리하는 중에 화면을 두 번 이상 터치하면 ANR이 발생**한다. (앞에 쌓여있는 Message를 먼저 처리하느라 터치 이벤트에 대한 처리가 지연되는 것)
- Service나 BroadcastReceiver에서도 5초 이내로 Message 처리가 필요
    - 예를 들어 50초 동안 BroadcastReceiver의 onReceive()가 실행되고 있을 때, Activity 화면을 터치하면 역시 ANR 발생 가능성이 높다. BroadcastReceiver나 Service도 Activity가 떠있는 상태를 고려해서, 타임아웃을 5초라고 생각하는 편이 낫다. 결론적으로 **BroadcastReceiver의 경우에 오래 걸리는 작업이 있다면 Service로 넘겨서 실행해야 하고, Service에서는 다시 백그라운드 스레드를 이용**해야 한다.