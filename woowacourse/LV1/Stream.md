# Stream (스트림)

## 스트림이란?

_(모던 자바 인 액션 참고)_

### `데이터 처리 연산`을 지원하도록 `소스`에서 추출된 `연속된 값 요소`

**연속된 요소**

    컬렉션과 마찬가지로 스트림은 특정 요소 형식으로 이루어진 연속된 값 집합의 인터페이스를 제공

    컬렉션의 주제 : 데이터
    -> 시간과 공간의 복잡성과 관련된 요소 저장 및 접근 연산이 주를 이룸
       ex) ArrayList를 사용할지, LinkedList를 사용할지

    스트림의 주제 : 계산
    -> filter, sorted, map 처럼 표현 계산식이 주를 이룸

**소스**

    스트림은 컬렉션, 배열, I/O 자원 등의 데이터 제공 소스로부터 데이터 소비

    스트림의 요소는 순서를 유지함
    -> 정렬된 컬렉션으로 스트림 생성시, 정렬이 그대로 유지

**데이터 처리 연산**

    함수형 프로그래밍 언어에서 일반적으로 지원하는 연산, 데이터베이스와 비슷한 연산 지원
    ex) filter, map, reduce, find, match, sort, ... 로 데이터 조작 가능

    스트림 연산은 순차적(stream) 또는 병렬(parallelStream)로 실행할 수 있음

### 🌟 스트림은 데이터 컬렉션 반복을 `멋지게` 처리하는 기능!

---

## 스트림의 특징

**선언형** : 더 간결하고 가독성이 좋아짐

**조립할 수 있음** : 유연성이 좋아짐

**병렬화** : 성능이 좋아짐

---

## 스트림은 왜 쓰는걸까?

### 1. HOW 중심의 코드가 아닌 `WHAT 중심의 코드`를 짜기 위해

예시 : 연봉 1억이 넘는 직장인들의 연봉 평균 구하기

#### 🙅‍♀️ BEFORE JAVA8 : 외부 반복

```JAVA
int sum = 0;
int count = 0;
for (Employee emp : emps) {
    if (emp.getSalary() > 100_000_000) {
        sum += emp.getSalary();
        count++;
    }
}

double average = (double) sum / count;
```

==

    Employee 목록 emps에서 각 Employee인 emp마다
    salary가 1억이 넘으면
    sum에 salary를 더하고
    count를 1 증가시켜야 해

    목록을 다 돌면 sum과 count로 평균을 구해줘

#### 🙆‍♀️ ARTER JAVA8 : 내부 반복

```JAVA
double average = emps.stream()
                    .filter(emp -> emp.getSalary() > 100_000_000)
                    .mapToInt(Employee::getSalary)
                    .average()
                    .orElse(0);
```

==

    Employee 중에서
    salary가 1억보다 큰 Employee
    의 salary를 구해
    salary의 평균을 구해줘

### 2. 불필요한 연산을 진행하지 않기 위해 (쇼트 서킷)

예시 : 밀가루만 먹는 손님에게 밀가루를 모르는 요리사가 요리해주기

#### 🙅‍♀️ 컬렉션 요리사

    손님 : 밀가루 요리 주세요
    요리사 : (밀가루가 뭐지?) 네~ 잠시만요!

    [케이크가 나왔습니다.]
    [쌀국수가 나왔습니다.]
    [샐러드가 나왔습니다.]
    [스테이크가 나왔습니다.]

    손님 : ?? 밀가루만 먹는다고요. 🤬

#### 🙆‍♀️ 스트림 요리사

    손님 : 밀가루 요리 주세요
    요리사 : (밀가루가 뭐지?) 네~ 잠시만요!

    [케이크가 나왔습니다.]

    손님 : 냠냠

    [쌀국수가 나왔습니다.]

    손님 : ?? 밀가루만 먹는다고요. 🤬

결과적으로, 손님은 떠났지만 요리를 적게 만든 스트림 요리사의 손해가 적음

    컬렉션 : 적극적 생성
    -> 자료구조가 포함하는 모든 값을 메서드에 포함시킴

    스트림 : 게으른 생성
    -> 요청할 때만 요소를 계산하는 고정된 자료구조를 가짐

    스트림은 여러개의 조건이 중첩된 상황에서
    값이 결정나면 불필요한 연산을 진행하지 않고 조건문을 빠져나옴
    -> 실행 속도를 높임
    = 쇼트 서킷

    단, 특정 연산자를 사용할 때만 적용됨!
    ex) allMatch, anyMatch, noneMatch, findFirst, findAny, limit, ...

---

## 스트림 사용 방법

### 스트림의 구조

#### 첫번째 : 스트림 생성

    stream()

#### 두번째 : 중간 연산

    filter(), map()

#### 세번째 : 최종 연산

    collect()

#### 파이프라이닝

스트림에서는 해당 부분들을 `.`으로 이어 사용함

```JAVA
stream().filter().map().collect()
```