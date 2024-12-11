# 이상 현상(Anomaly)

### 정의

- 특정 릴레이션에 대한 데이터를 조작함에 있어 원치 않는 현상이 발생하거나
- 조작으로 인하여 데이터 불일치/누락이 발생하는 현상
- 불필요한 데이터 중복으로 인해 릴레이션에 대한 데이터 삽입/수정/삭제 연산을 수행할 때 발생할 수 있는 부작용

### 발생 원인

- **데이터가 중복**되면서 생김
  - 여러 가지 사실들을 하나의 릴레이션으로 표현
- 속성들 간에 존재하는 여러가지 종속관계에 대하여 **<span style="background-color: yellow;">정규화</span>** 되지 않음

### 해결 방안

- 속성(attribute)들 간의 종속성(dependency)을 분석해서 하나의 릴레이션에는 하나의 종속성이 표현되도록 분해 -> 스키마 변환 (schema transformation)
  - ✨**정규화**✨

<br>

## 이상 현상 종류

![](/Database/images/anomaly_ex.jpg)

### 삽입 이상 (Insertion Anomaly)

- 새 데이터를 삽입하기 위해 불필요한 데이터도 함께 삽입해야 하는 문제

### 갱신 이상 (Modification ANomaly)

- 중복 튜플 중 일부만 변경하여 데이터가 불일치하게 되는 모순의 문제

### 삭제 이상 (Deletion Anomaly)

- 튜플을 삭제하면 꼭 필요한 데이터까지 함께 삭제되는 데이터 손실의 문제

> 삽입, 갱신, 삭제에만 이상이 생김, 조회는 X

<br>

## 정규화

- 2 정규화 (부분함수 종속성 해결)
  - 기본키에 완전 함수 종속이 되지 않는 등급과 할인율 속성 때문

![](/Database/images/anomaly_ex2.jpg)
![](/Database/images/anomaly_ex3.jpg)

<br>

- 3 정규화 (이행함수 종속성 해결)
  - 이상 형상이 발생하는 원인은 이행 함수적 종속이 존재하기 때문

![](/Database/images/anomaly_ex4.jpg)
![](/Database/images/anomaly_ex5.jpg)
![](/Database/images/anomaly_ex6.jpg)
