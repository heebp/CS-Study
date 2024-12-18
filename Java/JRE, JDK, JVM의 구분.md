JRE, JDK, JVM의 구분
=============

## 1. JRE
- JVM과 자바 프로그램을 실행시킬 때 필요한 라이브러리들을 함께 가지고 있는 패키지

## 2. JDK
> JDK란?
  - 자바 개발 키트라고 불리며 Java 환경에서 돌아가는 프로그램을 개발하는 데 필요한 툴을 모아놓은 소프트웨어 패키지
  - JRE, Java 바이트코드 컴파일러, Java 디버거 등을 포함
  ![JDK 이미지](/Java/images/jdk.png)

> JDK 주요 구성요소
  - javac : 자바 코드를 컴파일하여 바이트 코드로 변환하는 자바 컴파일러
  - JVM : 바이트 코드를 실행코드로 변환하고 실행
  - 자바 API 라이브러리 : 자바 개발을 위한 표준 API 라이브러리
  - 도구와 유틸리티 : 디버깅, 프로파일링, 패키지 관리 도구
  - 개발 문서와 예제 코드 : 공식 문서와 예제 코드

> JDK 특징
  - 플랫폼 독립성 : JDK로 개발하면 다양한 운영체제와 하드웨어에서 실행이 가능
  - 버전 관리 : 정기적인 업데이트 지원
  - 라이선스 : 무료 및 상용 버전 지원

## 3. JVM
> JVM이란?
  - Java로 개발한 프로그램을 컴파일하여 만들어지는 바이트코드를 실행시키기 위한 가상머신

> JVM 구조

  ![JVM 이미지](/Java/images/jvm.gif)
  - Class Loader(클래스 로더)
    - JVM 내로 클래스 파일을 동적으로 로드하는 모듈
    - 로드된 바이트 코드들을 묶어서 JVM의 메모리 영역인 Runtime Data Areas에 배치
    - 클래스 파일의 로딩 순서는 Loading - Linking - Initialization 순으로 진행
    - Loading은 클래스 파일을 JVM의 메모리에 로드하는 작업
    - Linking은 클래스 파일을 사용하기 위해 검증하는 작업
    - Initialization은 클래스의 변수들을 적절한 값으로 초기화하는 작업
  
  - Execution Engine(실행 엔진)
    - 클래스 로더를 통해 런타임 데이터 영역에 배치시킨 바이트 코드를 명령어 단위로 읽어서 실행한다.
    - JVM이 실행할 수 있는 상태로 변경해주는 작업
    - 인터프리터와 JIT 컴파일러를 혼합하여 실행
    - 인터프리터는 명령어를 하나씩 읽어서 해석하고 바로 실행
    - 반복되는 코드를 컴파일하여 네이티브 코드로 변경하고 캐싱 방식으로 실행하는 방식
    - 가비지 컬렉터를 통해 Heap 영역에 사용하지 않는 메모리를 자동으로 회수
   
  - Runtime Data Areas(런타임 데이터 영역)
    - Method area(메소드 영역)
      - 바이트 코드를 처음 메모리 공간에 올리기 위해 초기화되는 대상을 저장하기 위한 메모리 영역
      - 정적 필드와 클래스 구조를 가지고 있음
    - Heap(힙 영역)
      - 데이터를 저장하기 위해 런타임시 동적으로 할당하여 사용하는 영역
      - new 타입의 클래스나 인스턴스 변수, 배열과 같은 Reference Type이 저장
    - Java Stacks(스택 영역)
      - 임시적으로 사용되는 변수나 정보들이 저장되는 영역
      - 기본 자료형이 저장됨
    - PC Registers
      - 현재 수행중인 JVM 명령어 주소를 저장하는 영역
    - Native Method Stacks(네이티브 메소드 스택)
      - 실제 실행할 수 있는 기계어로 작성된 프로그램을 실행시키는 영역
  - Native Method Interface(네이티브 메소드 인터페이스)
    - 자바가 다른 언어로 만들어진 어플리케이션과 상호 작용할 수 있는 인터페이스를 제공해주는 프로그램
  - Native Method Library(네이티브 메소드 라이브러리)
    - 다른 언어로 작성된 라이브러리

### 참고 사이트
- https://namu.wiki/w/JDK
- https://inpa.tistory.com/entry/JAVA-%E2%98%95-JDK-JRE-JVM-%EA%B0%9C%EB%85%90-%EA%B5%AC%EC%84%B1-%EC%9B%90%EB%A6%AC-%F0%9F%92%AF-%EC%99%84%EB%B2%BD-%EC%B4%9D%EC%A0%95%EB%A6%AC
- https://namu.wiki/w/Java%20Virtual%20Machine
- https://velog.io/@itonse/JVM2-JDK-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0
- https://burningfalls.github.io/java/what-is-jdk/
- https://inpa.tistory.com/entry/JAVA-%E2%98%95-JVM-%EB%82%B4%EB%B6%80-%EA%B5%AC%EC%A1%B0-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%98%81%EC%97%AD-%EC%8B%AC%ED%99%94%ED%8E%B8
