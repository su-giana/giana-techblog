# 테스트는 어떻게 작동할까?
## 일단 파고들기 이전에 실행부터!
대부분의 테스트들은 `() -> {run(...); assertThat(...);}`을 첫 인자로 넣습니다. `assertThat`은 assertJ의 함수니까 이해하는데 run은 뭐하는 친구일까요?
![|300](https://i.imgur.com/IC4bFRb.png)
![|500](https://i.imgur.com/gX1PHpg.png)
흠! 입력으로 들어온 문자열들을 `System.in` 즉, 입력에 넣고 `runMain`을 작동시키네요. 우리가 정의하는 `runMain`은 아래와 같습니다. 
![](https://i.imgur.com/q5lTDkX.png)
그러니까, 입력을 넣고 main함수를 실행시키는거죠! 그러면 이제 본격적으로 내부로 들어갑시다.

---
## Mockito를 이해해 봅시다
### 왜 쓸까요? 🧐
> [!NOTE] 프로그래머가 직접 행동을 관리하기 위해 생성하는 객체입니다.
> 그러니까 테스트가 어려운 특정 상황 (우리같은 경우 Random 함수를 사용할 때)에서 해당 상황을 재현하기 위해 사용하는 **가짜 객체**를 우리는 **Mock**이라고 부릅니다.

### 그러면 어디서 쓰이는데요?
![](https://i.imgur.com/cazwT7I.png)
이 함수는 우리가 테스팅에 사용하는 `assertRandomNumberInRangeTest`에서 호출되는 함수입니다! try문의 조건절을 보면 우리가 찾던 Mock 객체가 보이네요.
![](https://i.imgur.com/rD8P87U.png)
1. 만약 우리가 `assertRandomNumberInRangeTest(...)`를 실행시킨다면 첫 인자에는, **`pickNumberInRange()`** 함수가 실행되었는지 여부가 Verification에 들어가네요. 
2. 이후에 RANDOM_TEST_TIMEOUT동안 테스트를 실행하는 `assertTimeoutPreemptively(...)`를 실행합니다.
3. 이후 try문에서 `MockStatic`형으로 `Randoms.class`에 `mockStatic()`을 적용한 결과를 반환합니다. `MockStatic`의 공식문서를 조금 더 자세히 살펴보면
   >Represents an active mock of a type's static methods. The mocking only affects the thread on which this static mock was created and it is not safe to use this object from another thread.
   
   라고 적혀있네요, 이번엔 `mockStatic`의 공식 문서를 봅시다.
   > Creates a thread-local mock controller for all static methods of the given class or interface.
   > 즉, 주어진 클래스나 인터페이스의 thread-local mock controller를 생성합니다.
   
4. `mock.when(verification).thenReturn(value, Arrays.stream(values).toArray())`는 verification이 충족될 때 mock 객체가 `value`, `values`를 실행시에 반환하게 한다는 뜻입니다.
5. ![](https://i.imgur.com/DEHSVkm.png)
   해당 예시를 들어볼까요? 
   이 경우에는 `Randoms`의 Mock 객체에 `MOVING_FORWARD`와 `STOP`을 반환하게 한 후, 입력을 "pobi,woni", "1"로 설정해서 `main` 함수를 실행하겠네요. 그리고 결과는 `contains()` 안의 값들을 포함해야 하고요. 

---
# 후기!
> [!NOTE] 역시 기술은 사람을 설레게 하는구만
> 새로운 코드를 뜯으면서 너무나 설렜습니다..! 특히 우아한 형제들에서 짠 코드는 어떨까 했는데, 역시 절 실망시키지 않네요 🥰. 더욱 정진해서 저도 멋진 프로그래머가 되겠습니다! 다른 테스트 메소드들을 사용할 일이 생긴다면, 글을 더 추가해볼게요.
