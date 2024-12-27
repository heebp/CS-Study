# CountDownLatch 사용법

**CountDownLatch**는 어떤 쓰레드가 다른 스레드에서 작업이 완료될 때까지 기다릴 수 있도록 해주는 클래스입니다.

---

## 작동 원리

아래 그림과 같이 `TA`는 `T1`, `T2`, `T3` 스레드를 호출하고 `cnt`가 0보다 크면 대기합니다.  
`T1`, `T2`, `T3`는 작업이 완료되면 `countDown()`를 호출해 `cnt` 값을 감소시킵니다.  
`cnt`가 0이 되면 차단된 스레드 `TA`가 해제되어 나머지 작업을 수행합니다.

---

## 기본 사용법

```java
// latch의 숫자를 입력
CountDownLatch countDownLatch = new CountDownLatch(5);

// latch 숫자 감소
countDownLatch.countDown();

// latch의 숫자가 0이 될 때까지 대기
countDownLatch.await();
```

---

## 다른 스레드 작업 대기하기

**CountDownLatch** 클래스를 이용해 Main 스레드가 다른 스레드의 작업이 완료될 때까지 기다리도록 코드를 작성해봅니다.

### Main 클래스

```java
public class Main {
    public static void main(String[] args) throws InterruptedException {
        Instant start = Instant.now();

        System.out.println("Start");
        int totalNumberOfTasks = 3;
        BlockingQueue<Integer> queue = new LinkedBlockingQueue<>(200);

        ExecutorService executorService = Executors.newFixedThreadPool(totalNumberOfTasks);
        CountDownLatch latch = new CountDownLatch(totalNumberOfTasks);

        executorService.submit(new Producer(queue, latch));
        executorService.submit(new Consumer(queue, latch));
        executorService.submit(new Consumer(queue, latch));

        executorService.shutdown();
        latch.await();

        Instant finish = Instant.now();
        long timeElapsed = Duration.between(start, finish).toMillis();

        System.out.println("Finished");
        System.out.println("Method took: " + timeElapsed + "ms");
    }
}
```

---

### Producer 클래스

```java
public class Producer implements Runnable {

    private final BlockingQueue<Integer> queue;
    private final CountDownLatch latch;

    public Producer(BlockingQueue<Integer> queue, CountDownLatch latch) {
        this.queue = queue;
        this.latch = latch;
    }

    @Override
    public void run() {
        try {
            process();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }

        latch.countDown();
    }

    private void process() throws InterruptedException {
        for (int i = 0; i < 100; i++) {
            System.out.println("[Producer] Put : " + i);
            queue.put(i);
            System.out.println("[Producer] Queue remainingCapacity : " + queue.remainingCapacity());
            Thread.sleep(100);
        }

        queue.put(-1);
    }
}
```

---

### Consumer 클래스

```java
public class Consumer implements Runnable {

    private final BlockingQueue<Integer> queue;
    private final CountDownLatch latch;

    public Consumer(BlockingQueue<Integer> queue, CountDownLatch latch) {
        this.queue = queue;
        this.latch = latch;
    }

    @Override
    public void run() {
        try {
            while (true) {
                Integer take = queue.take();
                if (take == -1) {
                    queue.put(-1);
                    break;
                }
                process(take);
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }

        latch.countDown();
    }

    private void process(Integer take) throws InterruptedException {
        System.out.println("[Consumer]  Take : " + take);
        Thread.sleep(500);
    }
}
```

---

## 실행 결과

```
Start
[Producer] Put : 0
[Producer] Queue remainingCapacity : 199
[Consumer]  Take : 0
[Producer] Put : 1
...
[Consumer]  Take : 196
[Consumer]  Take : 197
[Consumer]  Take : 198
[Consumer]  Take : 199
Finished
Method took: 33884ms
```

---

## 요약

- **CountDownLatch**는 특정 작업이 완료되기를 기다릴 때 유용합니다.
- `countDown()` 메서드로 카운트를 줄이고, `await()` 메서드로 대기합니다.
- 여러 스레드 간 작업 조율에 효과적이며, 병렬 작업의 완료를 간단히 처리할 수 있습니다.

# CyclicBarrier 사용법

**CyclicBarrier**는 **CountDownLatch**와 매우 유사하지만, 다음과 같은 차이점이 있습니다:

- **CountDownLatch**: 각 스레드가 `countDown()`을 호출한 후에 `await()`을 만나지 않는다면 대기 상태에 빠지지 않고 자신의 할 일을 계속합니다.
- **CyclicBarrier**: 모든 스레드가 대기 상태에 들어가야 대기가 끝납니다.

---

## CyclicBarrier 예제

### 예제 1: 기본 CyclicBarrier 테스트

```java
@Test
void cyclicBarrierTest() throws InterruptedException {
    CyclicBarrier barrier = new CyclicBarrier(5);
    ExecutorService service = Executors.newFixedThreadPool(5);
    for (int i = 0; i < 4; i++) {
        service.execute(() -> {
            try {
                barrier.await();
                System.out.println("bepoz");
            } catch (InterruptedException | BrokenBarrierException e) {
                e.printStackTrace();
            }
        });
    }
    Thread.sleep(100);
    assertThat(barrier.getNumberWaiting()).isEqualTo(4);
}
```

### 설명

- `barrier.getNumberWaiting()`: 현재 대기 중인 스레드 수를 반환합니다.
- `CyclicBarrier barrier = new CyclicBarrier(5);`: 5개의 스레드가 대기 상태에 들어가야 대기가 끝납니다.
- `CountDownLatch`와 달리 `CyclicBarrier`는 모든 스레드가 대기 상태에 진입해야 다음 로직으로 넘어갈 수 있습니다.

#### 주의점

`Thread.sleep(100)`을 추가한 이유는 `await()`이 호출되기도 전에 `assert` 문으로 넘어가버리는 상황을 방지하기 위해서입니다.
`for`문의 반복 횟수를 5로 설정하면 정상적으로 다음 로직이 실행됩니다.

---

### 예제 2: CyclicBarrier의 반복 대기

```java
@Test
void cyclicBarrierTest() throws InterruptedException, BrokenBarrierException {
    CyclicBarrier barrier = new CyclicBarrier(5);
    ExecutorService service = Executors.newFixedThreadPool(5);
    for (int i = 0; i < 5; i++) {
        service.execute(() -> {
            try {
                barrier.await();
                System.out.println("bepoz");
            } catch (InterruptedException | BrokenBarrierException e) {
                e.printStackTrace();
            }
        });
    }
    Thread.sleep(100);
    System.out.println("a");
    barrier.await();
    System.out.println("b");
}
```

### 설명

- `CountDownLatch`는 한 번 count를 모두 소진시키면 이후 `await()`을 만나도 바로 통과합니다.
- 반면, `CyclicBarrier`는 **다시 시작**됩니다.

#### 작동 방식

1. 위 코드에서 `System.out.println("a");`가 출력됩니다.
2. 이후 `await()`을 만나고, 다시 5개의 스레드가 `await()`을 호출할 때까지 대기 상태에 머뭅니다.

---

## 요약

- **CyclicBarrier**는 모든 스레드가 대기 상태에 들어가야 다음 작업을 진행할 수 있습니다.
- **CountDownLatch**는 count를 소진시키면 이후에는 바로 통과할 수 있습니다.
- CyclicBarrier는 반복적으로 대기 상태를 설정할 수 있으며, `await()`을 호출하는 스레드 수를 조정해 동작을 관리합니다.

