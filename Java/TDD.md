# TDD (Test Driven Development : 테스트 주도 개발)

## TDD 란?

    TDD = TFD(Test First Development) + 리팩터링

### TDD를 하는 이유

- 디버깅 시간을 줄여줌

- 동작하는 문서 역할을 함

- 변화에 대한 두려움을 줄여줌

---

## 프로그래밍 순서

<br>

### 요구사항 정리

    readme 파일 작성

### 코드 작성

Red to Green : test 코드를 통해 `Red(fail)`를 띄운 후 production 코드를 통해 `Green(pass)`를 만드는 순서

    1. test 코드 작성

        - 요구사항에 맞게 순서대로 단위별로 테스트 작성

        - 되도록 촘촘하게 작성
            ex) 숫자 1을 입력하는 경우, 숫자 2를 입력하는 경우를 따로 작성하여 두 케이스가 모두 pass할 수 있도록 구현

        - test 메서드 명을 한글로 작성해도 되지만, warning이 발생하여 처리해줘야 함
            + 메서드 명을 영어로 작성한 후 @DisplayName을 통해 메서드의 목적을 명시함
            + 메서드 명을 한글로 작성한 후 @SuppressWarnings를 통해 warning을 끔

        - 하나의 기능을 한번에 테스트하기 어렵다면, 메서드를 더 작게 나누어 tset 코드를 작성해 볼 수 있음

    2. production 코드 작성

        - production 코드는 최소한으로 test가 통과하도록 구현

        - 새로운 test 코드에 대한 Green을 만들 경우 이전 test 코드의 Green이 깨지지 않도록 구현

## etc

`그 외의 생각해 볼 만한 거리들..`

### 타입 추론

    Java는 기본적으로 강타입 언어로, 타입을 명시적으로 지정해줘야 함
    but, jdk 11 부터 var로 타입 추론을 제공하고 있음
    -> 정답이 따로 있는 것은 아니니, 각각 사용을 해보며 자신의 타입을 찾아보는 것을 추천

### final

    Java는 기본적으로 가변이고, 불변을 사용할 경우 final을 붙여 줘야함
    but, 최근에 나오는 언어들은 기본적으로 불변을 제공한 후, 가변을 사용할 경우 따로 처리가 필요함
    -> 왜 최근 언어들은 기본적으로 불변을 제공할지, 그 장점이 무엇일지 생각해보기

### commit

    1. commit도 비용이다.

    기능별로 commit을 해야하는 것은 맞지만,그렇다고 너무 잦은 commit도 좋지 않음
    -> 적절한 commit 포인트에 대해 고민해보기

    2. Red to Green에서의 test 코드와 production 코드는 함께 commit한다.

    이 commit은 기능과 관련된 것이므로 feat: 메세지를 사용한다.

### test code

    1. private 메서드에 대한 test는 어떻게 진행하면 좋을까?

    해당 메서드가 사용되는 public 메서드로 간접테스트가 가능한 경우 별도로 테스트를 진행하지 않아도 됨
    테스트가 필요할 경우 해당 메서드를 test 코드로 복사해서 테스트를 진행해 볼 수 있음
    -> 더 생각해보기..

    2. 테스트만을 위한 생성자를 만드는건 괜찮을까?

    해당 생성자가 prodcution 코드에서 사용이 된다면 괜찮음
    -> 그렇지 않을 경우는..?

    3. 한 메서드에 assert 문을 여러 개 사용하는 것은 괜찮을까?

    해당 메서드의 fail이 어디서 떴는지 알 수 없기 때문에 좋지 않음
    -> 중복의 경우 @ParameterizedTest, @ValueSource 를 이용해 해결할 수 있음