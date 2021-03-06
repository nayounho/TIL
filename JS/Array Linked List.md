# Array와 Linked List의 차이점

- 배열과 연결 리스트는 데이터를 나열한다는 점에서 비슷하다
- 배열과 연결 리스트는 엄연히 다르기 때문에 사용하기에 따라 프로그램의 성능이 달라지게 된다

## 배열(Array)

1. 배열은 데이터를 논리적인 순서에 따라 순차적으로 나열하기 때문에 물리적 주소 또한 순차적이다
2. 인덱스를 가지고 있기 때문에 원하는 데이터에 접근하는 속도가 빠르다
3. 배열은 데이터의 삽입/삭제에는 취약하다. 배열 특성상 데이터의 삽입/삭제가 이루어지면 삽입/삭제가 이루어진 위치의 다음부터 모든 데이터의 위치를 변경해야 한다.

## 연결 리스트(Linked List)

1. 연결리스트는 데이터를 논리적 순서에 따라 입력. 하지만 물리적 주소는 순차적이지 않다
2. 인덱스를 가지고 있지 않기 때문에 인덱스 대신 현재  위치의 이전 및 다음 위치를 기억하고 있다
3. 한번에 데이터 접근이 가능하지 않고 연결되어 있는 링크를 따라 가야만 접근이 가능. 배열에 비해 속도가 떨어진다.
4. 데이터 삽입/삭제는 논리적 주소만 바꿔주면 되기 때문에 데이터 삽이/삭제가 용이