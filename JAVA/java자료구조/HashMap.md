- HashMap은 HashTable을 기반으로 한다.
- java8에서 내부구조는 배열과 연결리스트 또는 트리의 조합이다.
- key,value 구조로 key값은 해싱함수에 넣어 인덱스를 산출한 후 해당 인덱스에 map데이터를 저장 한다.
	- Hasing 결과는 정수이다.
	- Hasing 함수의 결과는 동일한 경우가 발생할 수 있다. 이 경우에 [충돌]이 발생하는데 이때 충돌 횟수에 따라서 저장방식이 바뀐다.
	- 충돌 초기에는 연결리스트(linkedList)로 관리 한다 -> seperate chaining
	- 일정한 충돌 횟수가 초과하게 되면 연길리스트가 적합하지 않기에 red-balck tree로 저장을 관리 한다.
	- 자바8 내부구조를 봤을때 충돌 횟수가 7이상인 경우 레드블랙트리를 사용하는 것으로 확인됨.


참조: 
[https://lordofkangs.tistory.com/78]
[https://ysjee141.github.io/blog/jdk/java-hashmap/]