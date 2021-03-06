# CSS 선택자의 특정성

- 브라우저는 CSS 규칙의 특정성에 따라 요소에 표시할 스타일을 결정
- 다음을 기반으로 각 규칙에 대해 계산 `a,b,c,d`
    1. `a`는 인라인 스타일이 사용되고 있는지 여부입니다. 속성의 선언이 요소의 인라인 스타일이면 `a`는 1이고, 그렇지 않으면 0입니다.
    2. `b`는 ID 셀렉터의 수입니다.
    3. `c`는 클래스, 속성, 가상 클래스 선택자의 수입니다.
    4. `d`는 태그, 가상 요소 선택자의 수입니다.
- 왼쪽에서 오른쪽 순으로 각 열의 가장 높은 값을 비교합니다. 따라서 b열의 값은 c와 d열에 있는 값을 무시합니다.
- 따라서 `0,1,0,0`의 특정성은 `0,0,10,10`보다 큽니다.

- 동등한 특정성의 경우: 가장 마지막 규칙이 중요한 규칙으로 적용 됩니다.