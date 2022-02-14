# Hi, Thread [JAVA]

Assign: 한승현
Status: 승현의 Hi 자료실

# Thread

- 동작하고 있는 프로그램을 프로세스(Process)라고 한다. 보통 한 개의 프로세스는 한 가지의 일을 하지만, 스레드를 이용하면 한 프로세스 내에서 두 가지 또는 그 이상의 일을 동시에 할 수 있다.

```
1. Thread
2. Join
3. Runnable
```

## Thread

예제를 통해 알아보자. 스레드는 가장 간단하게 다음과 같이 만들 수 있다.

```java
public class Sample extends Thread {
    public void run() {
        System.out.println("thread run.");
    }

    public static void main(String[] args) {
        Sample sample = new Sample();
        sample.start();
    }
}
```

Sample 클래스가 Thread 클래스를 상속했다. Thread 클래스의 run 메서드를 구현하면 위 예제와 같이 `sample.start()` 실행 시 sample 객체의 run 메서드가 수행된다

> Thread 클래스를 상속받았기 때문에 start 메서드 실행 시 run 메서드가 수행된다. Thread 클래스는 start 실행 시 run 메서드가 수행되도록 내부적으로 동작한다.
> 

위 예제를 실행하면 “thread run.”이라는 문장이 출력될 것이다. 하지만 위의 예처럼 스레드가 하나인 경우는 스레드가 어떻게 동작하고 있는지 명확하지가 않다.

스레드의 동작을 확인할 수 있게 예제를 수정해보겠다.

```java
public class Sample extends Thread {
	int seq;

	public Sample(int seq) {
		this.seq = seq;
	}

	public void run() {
		System.out.println(this.seq + " thread start.");
		try {
			Thread.sleep(1000);
		} catch (Exception e) {
		
		}
		System.out.println(this.seq + " thread end.");
	}

	public static void main(String[] args) {
		for (int i = 0; i < 10; i++) {
			Thread t = new Sample(i);
			t.start();
		}
		System.out.println("main end.");
	}
}
```

총 10개의 스레드를 실행시키는 예제이다. 어떤 스레드인지 확인하기 위해서 스레드마다 생성자에 순번을 부여했다. 그리고 스레드 메서드(run) 수행 시 시작과 종료를 출력하게 했고 시작과 종료 사이에 1초의 간격을 설정(`Thread.sleep(1000)`)했다. 그리고 main 메서드 종료 시 “main end.” 를 출력하도록 했다.

결과는 다음과 같았다.

```
0 thread start.
4 thread start.
6 thread start.
2 thread start.
main end.
3 thread start.
7 thread start.
8 thread start.
1 thread start.
9 thread start.
5 thread start.
0 thread end.
4 thread end.
2 thread end.
6 thread end.
7 thread end.
3 thread end.
8 thread end.
9 thread end.
1 thread end.
5 thread end.
```

0번 스레드부터 9번 스레드까지 순서대로 실행되지 않았고 그 순서에는 어떠한 규칙도 없었기 때문에, 스레드는 순서에 상관없이 동시에 실행된다는 사실을 알 수 있었다. 또한, 스레드가 모두 종료되기 전 main 메서드가 종료되었다는 것도 알 수 있었다.

### Join

위 예제를 보면 스레드가 모두 수행되고 종료되기 전에 main 메서드가 먼저 종료되었다. 그렇다면, main 메서드가 모든 스레드의 종료를 기다린 뒤에 종료되기를 원하는 경우에는 어떻게 해야할까?

아래 예제를 보자.

```java
import java.util.ArrayList;

public class Sample extends Thread {
	int seq;

	public Sample(int seq) {
		this.seq = seq;
	}

	public void run() {
		System.out.println(this.seq + " thread start.");
		try {
			Thread.sleep(1000);
		} catch (Exception e) {
		
		}
		System.out.println(this.seq + " thread end.");
	}

	public static void main(String[] args) {
		ArrayList<Thread> threads = new ArrayList<>();
		for (int i = 0; i < 10; i++) {
			Thread t = new Sample(i);
			t.start();
			threads.add(t);
		}
		
		for (int i = 0; i < threads.size(); i++) {
			Thread t = threads.get(i);
			try {
				t.join();
			} catch (Exception e) {
			
			}
		}

		System.out.println("main end.");
	}
}
```

생성된 스레드를 담기 위해서 Thread 타입의 ArrayList 객체 threads를 만든 후 스레드 생성 시 생성된 객체를 threads에 저장했다. 그리고 main 메서드가 종료되기 전에 threads에 담긴 각각의 thread에 `join()` 메서드를 호출하여 스레드가 종료될 때까지 기다리도록 했다. 스레드의 `join()` 메서드는 스레드가 종료될 때까지 기다리게 하는 메서드이다.

위와 같이 변경한 후 프로그램을 실행하면 다음과 비슷한 결과가 나올 것이다.

```
0 thread start.
4 thread start.
6 thread start.
2 thread start.
~~main end.~~ // join() 메서드로 인해서 main 메서드가 먼저 종료되지 않는다.
3 thread start.
7 thread start.
8 thread start.
1 thread start.
9 thread start.
5 thread start.
0 thread end.
4 thread end.
2 thread end.
6 thread end.
7 thread end.
3 thread end.
8 thread end.
9 thread end.
1 thread end.
5 thread end.
main end.
```

“main end.” 라는 출력이 가장 마지막에 위치한 것을 볼 수 있다.

스레드를 사용해 코드를 작성할 때 가장 많이 실수하는 부분이 바로 스레드가 종료되지 않았는데 스레드가 종료된 줄 알고 그 다음 로직을 수행하게 만드는 것이다. 스레드가 종료된 후 그 다음 로직을 수행해야 할 때, 꼭 필요한 join 메서드를 기억하자.

### Runnable

보통 스레드 객체를 만들 때 위의 예처럼 Thread를 상속하여 만들기도 하지만, 보통은 Runnable 인터페이스를 구현하도록 하는 방법을 주로 사용한다. 나중에 상속을 말하면서 다루겠지만, Thread 클래스를 상속하면 다른 클래스를 상속할 수 없기 때문이다.

위의 예제를 Runnable 인터페이스를 구현하는 방식으로 변경해보겠다.

```java
import java.util.ArrayList;

public class Sample implements Runnable {
	int seq;
	
	public Sample(int seq) {
		this.seq = seq;
	}

	public void run() {
		System.out.println(this.seq + " thread start.");
		try {
			Thread.sleep(1000);
		} catch (Exception e) {

		}
		System.out.println(this.seq + " thread end.");
	}

	public static void main(String[] args) {
		ArrayList<Thread> threads = new ArrayList<>();
		for (int i = 0; i < 10; i++) {
			Thread t = new Thread(new Sample(i));
			t.start();
			threads.add(t);
		}
		
		for (int i = 0; i < threads.size(); i++) {
			Thread t = threads.get(i);
			try {
				t.join();
			} catch (Exception e) {
			
			}
		}

		System.out.println("main end.");
	}
}	
```

Thread를 extends 하던 것에서 Runnable을 implements 하도록 변경되었다.

> Runnable 인터페이스는 run 메서드를 구현하도록 강제한다.
> 

그리고 Thread를 생성하는 부분을 다음과 같이 변경했다.

```java
Thread t = new Thread(new Sample(i));
```

Thread의 생성자로 Runnable 인터페이스를 구현한 객체를 넘길 수 있는데 이 방법을 사용한 것이다. 이렇게 변경된 코드는 이전에 만들었던 예제와 완전히 동일하게 동작한다. 다만, 인터페이스를 이용했으니 상속 및 기타 등등에서 좀 더 유연한 프로그램으로 발전했다고 볼 수 있겠다.

> 참고자료: [https://wikidocs.net/230](https://wikidocs.net/230)
>