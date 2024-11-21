## 오늘의 공부 내용

### 이펙티브 자바 3rd Edition - 아이템 58(전통적인 for문보다는 for-each 문을 사용하라)

```java
// 컬렉션 순회하기
for (Iterator<Element> i = c.iterator(); i.hasNext(); ) {
  Element e = i.next();
  // e로 비즈니스 로직 수행
}

// 배열 순회하기
for (int i = 0; i < a.length; i++) {
  // a[i]로 무언가를 한다.
}
```

반복자와 인덱스 변수는 모두 코드를 지저분하게 할 뿐이고, 진짜 필요한 건 📌 원소들뿐이다. 또한, 잘못된 변수를 사용했을 때 컴파일러가 잡아주리라는 보장도 없고, 컬렉션이냐 배열이냐에 따라 코드 형태가 상당히 달라지므로 주의해야 한다.

이상의 문제는 `for-each`문을 사용하면 모두 해결된다. 참고로 for-each 문의 정식 이름은 향상된 for문이다. 하나의 관용구로 컬렉션과 배열을 모두 처리할 수 있어서 어떤 컨테이너를 다루는지는 신경 쓰지 않아도 된다.

또한, 컬렉션을 중첩해서 순회해야 한다면 for-each 문의 이점이 더욱 커진다.

```java
enum Suit { CLUB, DIAMOND, HEART, SPADE }
enum Rank { ACE, DEUCE, THREE, FOUR, FIVE, SIX, SEVEN, EIGHT, NINE, TEN, JACK, QUEEN, KING}

static Collection<Suit> suits = Arrays.asList(Suit.values());
static Collection<Rank> ranks = Arrays.asList(Rank.values());

List<Card> deck = new ArrayList<>();
for (Iterator<Suit> i = suits.iterator(); i.hasNext(); )
  for (Iterator<Rank> j = ranks.iterator(); j.hasNext(); )
    deck.add(new Card(i.next(), j.next()));

// next()는 숫자(Suit) 하나당 한 번씩만 불려야 하는데, 안쪽 반복문에서 호출되는 바람에 카드(Rank) 하나당 한 번씩 불리고 있다. 그래서 숫자가 바닥나면 반복문에서 NoSuchElementException을 던진다.
```

그런데, for-each 문을 중첩하는 것으로 이 문제를 간단히 해결할 수 있다.

```java
for (Suit suit : suits)
  for (Rank rank : ranks)
    deck.add(new Card(suit, rank));
```

> [!WARNING]
> 하지만 안타깝게도 사용할 수 없는 상황이 세 가지 존재하는데, 🔫 파괴적인 필터링: 컬렉션을 순회하면서 선택된 원소를 제거해야 하는 경우에는 반복자의 remove 메서드를 호출해야 하는 경우, 🔫 변형: 리스트나 배열을 순회하면서 그 원소의 값 일부 혹은 전체를 교체해야 한다면 리스트의 반복자나 배열의 인덱스를 사용해야 하는 경우, 🔫 병렬 반복: 여러 컬렉션을 병렬로 순회해야 한다면 각각의 반복자와 인덱스 변수를 사용해 제어해야 하는 경우가 있다.

for-each문은 컬렉션과 배열은 물론 Iterable 인터페이스를 구현한 객체라면 무엇이든 순회할 수 있으므로 상황에 따라 사용하면 충분히 도움이 된다. 정리하자면, `for-each`문은 명료하고, 유연하고, 버그를 예방해준다.
