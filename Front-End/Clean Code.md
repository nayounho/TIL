# Clean Code

## Clean Code의 정의
> 코드가 우아하다는 의미는 군더더기 없이 깔끔하다는 의미이고, 효과적이라는 것은 기능을 수행하는 코드를 최대한 작은 라인으로 구현한다는 의미로 보면 될 것 이다.
>  - C++의 창시자인 Bjarne Stroustrup -

- 클린코드에서 가장 중요한 요소 중 하나는 가독성!
- 즉, 모든 팀원이 이해하기 쉽도록 작성된 코드
- 일반적으로 기존 코드를 변경하고자 할 때, 해석하는 시간과 수정하는 비율을 10:1 이라고 한다.
  - 예를 들어 코드를 변경하는 전체의 시간이 10시간이라고 한다면 해당 코드를 분석하는 시간이 9시간이며, 수정하는 시간은 단 1시간이 걸린다는 것이다.
- 그렇다면 해석이 어려운 코드는 그 만큼 분석에 소요되는 시간이 오래 걸리게 될 것이다. 
- 더욱이 대부분의 결함은 기존 코드를 수정하는 과정에서 발생한다고 하니 이해하기 쉬운 코드야말로 오류의 위험성을 최소화하는 것

## Technical Debt의 발생 원인 

<img src="./../Image/technical%20debt.png" width="600px" height="600px" title="px(픽셀) 크기 설정" alt="Technical Debt"></img>

- 이 그래프는 변경 비용과 대응속도에 대한 이상치와 실제 프로젣트에서 발생하는 수치를 비교한 그래프 이다.
- 아래쪽의 그래프에서 이상적인 변경비용은 일관되게 낮지만 실제 변경비용은 증가하는 것을 볼 수 있다.
- 위쪽 그래프는 시간이 지남에 따라 고객이 요구하는 대응 속도로, 이상적인 대응 속도는 높지만 실제 대응 속도는 점점 낮아지는 것을 볼 수 있다.

- 이러한 차이가 발생하는 이유는 프로젝트 초기에 클린코드로 개발하기 보다는 좀 더 빠르고 쉬운 방법으로 개발하기 때문이다. 
- 이렇게 쉽고 빠른 개발의 대표적인 것이 'copy & paste'이다.
- 이로 인하여 이상적인 변경 비용과 실제 변경 비용의 차이가 나는 부분이 바로 Technical Debt(기술 부채) 이다.

## Clean Code 작성 원칙 (일반적인 사항)
1. Follow Standard Conventions: Coding 표준, 아키텍쳐 표준 및 설계 가이드를 준수하라.
2. Keep It Simple, Stupid: 단순한 것이 효율적이며, 복잡함을 최소화 하라.
3. Boy Scout Rule: 캠핑장을 떠나기 전에 원래보다 깨끗하게 해야한다.(참조되거나 수정되는 코드는 원래보다 clean하게 해야 한다.)
4. Root Cause Analysis: 항상 근본적인 원인을 찾아라. 그렇지 않으면 반복될 것이다.
5. Do Not Multiple Languages in One Source File: 하나의 파일은 하나의 언어로 작성하라.

## Clean Code 작성 원칙 (설계 과점 SOLID원칙)
1. S(Simple Responsibility Principie) : 하나의 클래스는 하나의 책임만 가져야 한다.
2. O(Open/Closed Principle) : 클래스는 확장에 대하여 열려 있어야 하고, 변경에 대해서는 닫혀 있어야 한다.
3. L(Liskov Subvtitution Principle) : 파생 클래스의 메소드는 기반 클래스의 메서드를 대체하여 사용될 수 있어야 한다.
4. I(Interface Segregation Principle) : 클라이언트가 사용하지 않는 메소드에 의존하지 않아야 한다.
5. D(Dependency Inversion Principle) : 추상화된 것은 구체적인 것에 의존하면 안된다.

## 회고
- 사실 나는 클린 코드라는 말만 들어 봤지 실제로 어떠한 컨벤션에 의해서 사용을 하고 리펙토링에 큰 시간을 투자하지 못했었다.
  현재 회사에서 진행하고 있는 프로젝트도 프론트엔드 개발자가 나 혼자이기 때문에 팀원과 변수나 함수에 관한 컨벤션을 합의하는 
  과정이 없었다. 그래서 중요하게 생각하지 못한 부분도 있고 이미 기능적으로 완료된 함수나 변수를 리펙토링하기가 머리가 아팠다.
  그런데 새로운 프로젝트를 진행하면서 이전에 진행했던 프로젝트의 코드를 변경하고 부분적으로 수정하여 그래도 사용하는 상황이 
  왔을 때 clean code의 중요성을 인식하게 되었다. 그래서 지금 이 글을 쓰고 있는 것이기도 하다. 나 혼자만의 세계에 갖혀 
  기능이 우선이 되는 bad code를 작성하고 있었던 것이다. 컴포넌트 하나 가져다 쓸 수 없는 코드를 보다보니 React를 쓰는 의미마저
  없어지려고 하는 것을 보니 이번 프로젝트부터는 시간이 더 걸리더라도 clean code에 중점을 두고 개발을 해보도록 하겠다.


