# 아이템 12. toString을 항상 재정의하라

## Object의 기본 toString 메서드

Object.toString() 의 반환 값은 `클래스_이름@16진수로_표시한_해시코드`이다.

> ex) Class@232204a1

## toString의 일반 규약

- `간결하면서 사람이 읽기 쉬운 형태의 유익한 정보`를 반환해야 한다.
- 모든 하위 클래스에서 이 메서드를 재정의하라.

## 왜? 구현하는가

toString이 잘 구현한 클래스는 사용하기에 훨씬 즐겁고(?),
그 클래스를 사용한 시스템은 `디버깅`하기 쉽다.

## 어떻게 구현하는가

- 그 객체가 가진 주요 정보 모두를 반환하는게 좋다.
  - but, 객체가 거대하거나 문자열로 표현하기 적합하지 않다면 요약 정보를 담는다.
- 반환값의 포맷을 문서화할지 정한다.
  - 포맷을 명시했을 때
    - 장점
      - 표준적이고 명확하고 사람이 읽을 수 있다.
      - CSV파일 처럼 사람이 읽을 수 있는 데이터 객체로 저장할 수 있다.
      - 명시한 포맷에 맞는 문자열과 객체를 상호 전환할 수 있는 정적 팩터리나 생성자를 제공할 수 있다.
    - 단점
      - 그 포맷에 얽매이게 된다.
  - 포맷을 명시하지 않았을 때
    - 장점
      - 향후 릴리스에서 정보를 더 넣을 수 있다.
      - 포맷을 개선할 수 있는 유연성이 있다.
- 포맷 명시와 여부와 상관없이 toString이 반환한 값에 포함된 정보를 얻어올 수 있는 API를 제공하자.
- 추상클래스는 toString을 재정의 해주자. (하위클래스에서 사용할 수 있도록)
- `AutoValue`가 클래스의 의미까지는 파악하지 못한다.

_포맷을 명시하든 아니든 의도는 명확히 밝혀야 한다._

## 마무리

[Google의 AutoValue](https://github.com/google/auto)를 쓰는 것도 하나의 방법.
