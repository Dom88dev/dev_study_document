# DSL(Domain Specific Language)

DSL(Domain Specific Language)에 대한 간략 정의 페이지.

# DSL이란

특정 도메인(산업 등 특정 영역)에 특화된 언어를 말한다.

특정 영역의 문제 해결에는 그 영역에 맞는 특화된 도구를 사용하고 표현 방식은 해당 도메인의 전문가가 이해할 수 있는 형태(고급언어)여야 한다.

UNIX에서 특정한 응용 영역의 문제를 해결하기 위해 그 영역에만 적응할 수 있는 특수한 언어를 만들어 문제를 해결하는 전통이 존재 → 이러한 언어를 'little language' 또는 'mini language'라고 부르는데 DSL도 이와 유사하다.

# 내부 DSL과 외부 DSL

### 내부 DSL

- 호스트 언어 구문을 이용해 자체적으로 의존하는 무언가를 만드는 경우
- API와 DSL의 경계가 모호해 비슷하게 생각하는 경향이 있음

    → 일반 사용자가 알아보기 쉬운 API가 내부 DSL로 생각해도 될 듯함

- 호스트 언어 능력과 지금까지 사용하던 도구를 그대로 사용할 수 있음
- 처리 결과를 쉽게 예측할 수 있어 해당 언어를 잘 알면 친근할 수 있음
- 형태
    - 메타 프로그래밍의 형태로 언어에 'mini language'를 만들 수 있음
    - 원래 언어가 새로운 구문으로 도입됨 → 언어 확장을 일으켜 다른 언어가 됨
    - 인라인 코드 형태로 표현될 수도 있음
- 적합한 언어

    Lisp, Ruby, Smalltalk...

### 외부 DSL

- 호스트 언어와 다른 언어(XML, Makefile과 같은 고유 형식)에서 생성된 DSL
- GUI 도구를 제공해주는 것이 특징
- DSL과 범용 언어(GPL : General Purpose Language)와의 경계가 모호해지는 경향이 있음

    → 차이점은 언어 작성자와 사용자의 목적에 있음 

    특정 영역에서 언어의 작성자가 아닌 사용자의 목적에 부합하며 이해를 할 수 있으면 외부 DSL

- DSL 개발자가 DSL의 형식을 자유롭게 결정
- 형태
    - 실행 파일에서 DSL을 동적으로 로딩할 수 있음
    - DSL 컴파일러를 만들어서 표현할 수 있음
    - DSL을 범용언어 코드로 변환
- 적합한 언어

    Java, C#, C++...

# DSL의 장단점

### 장점

- 반복이 제거되고 비슷한 처리 코드는 자동 생성(템플릿)
- 프로그래밍 코드의 양이 적고 가독성이 높음
- 특정 프로그래머(*lay programer* - martin fowler)들과 커뮤니케이션이 쉬움

    → XML, CSS, SQL 등

### 단점

- 설계가 어려움
- 설계가 잘 되지 않을 시 읽기 어려운 코드가 될 수 있음
- 하위 호환성을 유지해야함

# 대표적인 DSL

- Java

    ANT, Maven, struts-config.xml, Seasar2 S2DAO, HQL(Hibernate Query Language), JMock

- Ruby

    Rails Validations, Rails ActiveRecord, Rake, RSpec, Capistrano, Cucumber

- Kotlin

    Koin

- ETC

    SQL, CSS, Regular expression(정규식), Make, graphviz

참조 사이트
→ [DomainSpecificLanguage](https://martinfowler.com/bliki/DomainSpecificLanguage.html)
→ [DslBoundary](https://www.martinfowler.com/bliki/DslBoundary.html)