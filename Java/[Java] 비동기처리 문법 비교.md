# [Java] 비동기처리 문법 비교

> 비동기의 사전적 의미 : 동시에 일어나지 않음
동기의 사전적 의미 : 동시에 일어남
> 

> CS의 비동기 : 작업의 완료를 기다리지 않고 다음 작업으로 넘어가는 방식
> 

### 이해하기 쉬운 방식

- 동기적인 코드 : 하나의 시간(동일한 시간)에서 여러 작업들이 실행된다.
    
    ⇒ 하나의 작업이 끝나고 다음 작업이 실행
    
- 비동기적인 코드 : 여러 시간(동일하지 않는 시간)에서 여러 작업들이 실행된다.
    
    ⇒ 서로 독립적으로 실행 ⇒ 작업이 끝나기를 기다리지 않고 다른 작업을 동시에 실행
    
- 동기화: 두개 이상의 작업, 데이터, 시스템 간의 **상태를 일치**시키거나 조화롭게 만드는 과정

### 비동기 처리의 목적

- 리소스 최적화 : I/O 상황에서 CPU가 쉬지 않고 다른 작업을 수행

### 동기화가 필요없는 메커니즘

- 재진입 코드 또는 순수 코드
    - 실행 중간에 다른 코드를 수행하고 와도 상관없는 코드
- 스레드 로컬 저장소
    - 한 스레드 내에서만 데이터가 공유되도록 보장
    
    ex) 웹 서버
    

### 동기화가 필요한 매커니즘

- volatile 키워드
    
    ```java
    private volatile Test test; //인스턴스 변수 또는 클래스 변수 앞에 사용
    ```
    
    - 가시성을 보장 : 모든 스레드에서 이 변수를 투명하게 볼 수 있음
    - 명령어 재배치 방지 : 최적화하면서 발생하는 명령어 재배치를 방지 → 실행 순서 보장
    - 한 스레드가 값을 수정하면 다른 스레드들도 새로운 값을 즉시 알게됨
        - How?
            
            CPU 캐시(레지스터)를 읽지 않고 메인 메모리 조회
            
- synchronized 키워드
    
    ```java
        private int count = 0;
    
        public synchronized void increment() {
            count++;
        }
    
    ```
    
    - 코드 블럭의 원자성을 보장

## **비동기 처리가 유용한 경우**

- I/O 작업
    - 파일 읽기/쓰기, 네트워크 요청 등 시간이 오래 걸리는 작업.
- UI
    - 사용자 입력을 처리하면서 동시에 백그라운드 작업을 수행.
- 병렬처리
    - 대량의 데이터를 병렬로 처리하여 속도를 높임

## 자바 비동기 처리

### Thread

가장 기본적인 비동기 처리 방식.

```java
Thread thread = new Thread(() -> {
    sout("비동기 작업 수행 중...");
});
thread.start();
System.out.println("작업1");

```

- 결과

```java
작업1
비동기 작업 수행 중...
```

### ExecutorService

- 스레드 풀을 사용하여 수를 제한하고 재사용 가능
- shutdown과 같은 관리가 필요

```java
// 이전에 만들어진 스레드 중에서 사용 가능한 게 있으면 재사용
ExecutorService executor = Executors.newCachedThreadPool();
        
//긴 작업을 다른 스레드에게 맡기고 
//메인 스레드는 작업 2를 실행한다.
executor.submit(() -> {
	sout("긴 작업 시작");
	try {
		Thread.sleep(1000);
	} catch (InterruptedException e) {
		e.printStackTrace();
	}
	sout("긴 작업 종료");
});

sout("작업 2 시작");
try {
	Thread.sleep(500);
} catch (InterruptedException e) {
	e.printStackTrace();
}
sout("작업 2 종료");

executor.shutdown();
```

- 결과

```java
작업 2 시작
긴 작업 시작
작업 2 종료
긴 작업 종료
```

### Future

- 비동기 작업의 결과를 처리
- ExecutorService와 함께 사용
- 작업 완료 후 결과를 가져옴
- 결과를 받기 위한 코드에서 블로킹 발생

```java
ExecutorService executor = Executors.newSingleThreadExecutor();

Future<String> future = executor.submit(() -> {
    Thread.sleep(1000);
    return "작업 완료!";
});

try {
    String result = future.get(); // 블로킹
    System.out.println(result);
} catch (Exception e) {
    e.printStackTrace();
}

executor.shutdown();
```

- 결과

```java
다른 코드...
작업 완료!
```

### CompletionHandler

- 비동기 채널에서 사용되는 콜백 인터페이스
- 비동기 작업의 결과를 처리
- 논 블로킹 방식

```java
CompletionHandler<Integer, Void> handler = new CompletionHandler<>() {
	@Override
	public void completed(Integer result, Void attachment) {
		System.out.println("성공: " + result + " 바이트가 쓰여졌습니다.");
	}
	@Override
	public void failed(Throwable exc, Void attachment) {
		System.err.println("실패: " + exc.getMessage());
	}
};
channel.write(buffer, 0, null, handler);
```

### CompletableFuture

- 메서드 체이닝 사용 가능
- 논 블로킹 방식

```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
	// 비동기 작업
	try {
		Thread.sleep(1000);
	} catch (InterruptedException e) {
		throw new IllegalStateException(e);
	}
	return "작업";
}).thenApply(result -> result + " 완료");

// 작업 완료 후 처리
future.thenAccept(result -> System.out.println("결과: " + result));

// 주 스레드가 종료되지 않도록 대기
future.join(); //블로킹?
```

- 결과

```java
결과: 작업 완료
```

### **멀티스레딩 vs 논블로킹**

| **특징** | **멀티스레딩** | **논블로킹** |
| --- | --- | --- |
| **스레드 수** | 요청마다 별도의 스레드 사용. | 적은 수의 스레드로 다수의 요청 처리. |
| **자원 효율성** | 블로킹 시 스레드가 대기하므로 자원 소모가 큼. | 블로킹 없이 작업을 진행하므로 자원 효율적. |
| **컨텍스트 스위칭 비용** | 스레드 간 전환(Context Switching) 비용 발생. | 단일 스레드에서 처리하므로 스위칭 비용 적음. |

### 활용 예

1. 사용자가 프로필 닉네임을 바꾸면 
2. 이전 닉네임을 로그에 저장하고
3. 새 닉네임으로 업데이트하는 로직
- MySQL

```java
		insert into nickname_logs(
			select id, nickname, now() from users where id = user_id
		);
		update users set nickname = new_nick where id = user_id;
```

- 자바 원본 코드

```java
//UserService.java
...
public void updateNickname(Long userId, String newNick){
	User user = userRepository.findbyId(userId);
	logRepository.save(user);
  user = User.builder()
	           .id(user.getId())
             .nickname(newNick)
             .build();
  userRepository.save(user);
}
```

- 개선된 코드 (비동기로 변경)

```java

    CompletableFuture<Void> logTask = CompletableFuture.runAsync(
	    () -> logRepository.save(user)
    );

   
    CompletableFuture<Void> userTask = CompletableFuture.runAsync(() -> {
        User updatedUser = User.builder()
                               .id(user.getId())
                               .nickname(newNick)
                               .build();
        userRepository.save(updatedUser);
    });

 
    CompletableFuture<Void> combinedTask = CompletableFuture.allOf(logTask, userTask);

    combinedTask.join(); 
```