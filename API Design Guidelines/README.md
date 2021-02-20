# Fundamentals

## Clarity at the point of use
* 사용 시점을 명확하게 잡는게 가장 중요한 목표다!
* 메소드나 프로퍼티는 한번 선언되어, 끊임없이 재사용 되어야 한다.
<br> (Design APIs는 이러한 사용을 명확하고 간결하게 도와줄 것이다.)
* 선언문을 읽어 보는 것에만 그치지 말고, 문법이 명확하게 돌아가는지 항상 문맥을 다 살펴야 한다.

## Clarity is more important than brevity
* 간결한 코딩 보다는, 확실히 알 수 있는 코드가 더 중요하다!
* 스위프트는 되게 콤팩트한 언어지만, 코드를 최대한 짧게 짜려는게 이 언어의 목표는 아니다.
* 짧게 짠 코드는 코드의 위험성을 높일 수 있기 때문에, 오히려 시스템에 강력한 부작용을 낳을 수 있다.

## Write a documentation comment
* 모든 선언에 확실한 주석처리를 진행해야 한다!
* 주석을 통한 확실한 코드이해는 코드설계에 큰 영향력을 가지므로 미루지 말자!

___
<br>

> 만약, 자신의 API를 짧은주석으로 담아내기가 어렵다면, API의 설계가 잘못 된 걸지도 모른다..!!
<br>

## Begin with a summary
* 보통, 선언문 자체로나 그 주석으로부터 API를 파악할 수 있게되므로,
<br>선언할 엔터티를 설명하는 요약부터 시작하자!
```
// Returns a "view" of `self` containing the same elements in
// reverse order.
func reversed() -> ReverseCollection
```

## Focus on the summary
