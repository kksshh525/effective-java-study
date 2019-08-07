> 아이템 85. 자바 직렬화의 대안을 찾으라

객체 직렬화란? 객체를 바이트 스트림으로 인코딩하고(직렬화) 그 바이트 스트림으로부터 다시 객체를 재구성하는(역직렬화) 메커니즘이다. 직렬화된 객체는 다른 VM에 전송하거나 디스크에 저장한후 나중에 역직렬화 할 수 있다. 이번 장은 직렬화가 품고 있는 위험과 그 위험을 최소화 하는 방법에 집중한다.



직렬화 근본적인 문제는 공격범위가 너무 넓고 지속적으로 더 넓어져 방어하기 어렵다는 점이다. 예를 들어 ObjectInputStream의 readObject 메서드는 객체 그래프가 역직렬화된다. readObject 메서드는 (Serializable 인터페이스를 구현했다면) 클래스 패스 안의 거의 모든 타입의 객체를 만들어 낼 수 있는, 사실상 마법 같은 생성자다. 바이트 스트림을 역직렬화 하는 과정에서 이 메서드는 그 타입들 안의 모든 코드를 수행할 수 있다. 그 말인 즉슨, 그 타입들의 코드 전체가 공격 범위에 들어간다는 뜻이다. 



자바의 표준 라이브러리나 아파치 커먼즈 컬렉션 같은 서드 파티 라이브러리는 물론 애플리케이션 자신의 클래스들도 공격 범위에 포함된다. 

> 신뢰할 수 없는 스트림을 역직렬화하면 원격 코드 실행, 서비스 거부 등의 공격으로 이어질 수 있다. 잘못한게 아무것도 없는 애플리케이션이라도 이런 공격에 취약해질 수 있다. 



결국, 직렬화 위험을 회피하는 가장 좋은 방법은 아무것도 직렬화 하지 않는 것이다. 



크로스 플랫폼 구조화된 데이터 표현의 선두주자는 JSON과 프로토콜 버퍼다. JSON은 더글라스 크록퍼드가 브라우저와 서버의 통신용으로 설계했고, 프로토콜 버퍼는 구글이 서버 사이에 데이터를 교환하고 저장하기 위해 설계했다. 보통은 이들을 언어 중립적이라고 하지만, JSON은 자바스크립트용으로, 프로토콜 버퍼는 C++용으로 만들어 졌고, 아직도 그 흔적이 남아 있다. 

둘의 가장 큰 차이는 JSON은 텍스트 기반이라 사람이 읽을 수 있고, 프로토콜 버퍼는 이진 표현이라 효율이 훨씬 높다는 점이다. 또한 JSON은 오직 데이터를 표현하는 데만 쓰이지만 프로토콜 버퍼는 문서를 위한 스키마(타입)를 제공하고 올바로 쓰도록 강요한다. 효율은 프로토콜 버퍼가 훨씬 좋지만 텍스트 기반 표현에는 JSON 이 아주 효과적이다. 또한, 프로토콜 버퍼는 이진 표현 뿐아니라 사람이 읽을 수 있는 텍스트 표현(pbtxt)도 지원한다. 




