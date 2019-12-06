# Koin DSL

Kotlin 덕분에, Koin은 앱을 설명하기 위해 앱에 주석을 달거나 코드를 생성하는 대신 그것을 돕는 DSL을 제공한다. Kotlin DSL과 함께, Koin은 의존성 주입을 이루기 위해 스마트한 기능적 API를 제공한다.

# Application & Module DSL

`KoinApplication`인스턴스는 Koin container 인스턴스의 구성으로 이를 통해 logging이나 properties 로딩 및 modules 를 구성하게 해준다.

새로운 `KoinApplication`을 빌드하기위해 다음 기능을 사용한다.

- `koinApplication { }` : `KoinApplication`의 컨테이너 구성을 만든다.
- `startKoin { }` : `KoinApplication`의 컨테이너 구성을 만들고 GlobalContext API 사용을 허용하기 위해 `GlobalContext`에 `KoinApplication`을 등록한다.

`KoinApplication`인스턴스의 구성을 위해 다음 기능들을 사용할 수 있다.

- `logger( )` : 사용할 레벨 및 로거 구현을 설명(기본적으로 EmptyLogger를 사용)
- `modules( )` : 컨테이너에 로드할 Koin modules 목록을 설정(list | vararg list)
- `properties()` : Koin 컨테이너에 해쉬맵 properties를 로드
- `fileProperties( )` : Koin 컨테이너에 주어진 파일의 properties를 로드
- `environmentProperties( )` : Koin 컨테이너에 OS 환경의 properties를 로드

# KoinApplication instance: Global vs Local

위와 같이 2가지 방법으로 Koin 컨테이너 구성을 묘사할 수 있다 : 함수 `koinApplication` | `startKoin`

컨테이너 구성을 GlobalContext에 등록하면 전역 API로 그것을 직접 사용할 수 있다.

모든 `KoinComponent`는 `Koin` 인스턴스를 나타낸다. 기본적으로 `GlobalContext`의 것을 사용한다.

# Starting Koin

Koin을 시작한다는 것은 GlobalContext로 KoinApplication 인스턴스를 실행한다는 의미.

모듈로 Koin 컨테이너를 시작하기 위해 startKoin 함수를 다음과 같이 사용할 수 있다.

    // start a KoinApplication in Global context
    startKoin {
        // declare used logger
        logger()
        // declare used modules
        modules(coffeeAppModule)
    }

# Module DSL

Koin 모듈은 앱에 주입/합성 될 정의들을 모은다. 새 모듈을 만들 때 다음 함수를 사용한다.

- `module { // module content }` : Koin 모듈 생성

모듈에서 컨텐츠를 나타내기 위해 다음 함수들을 사용할 수 있다.

- `factory { // definition }` : factory bean 정의를 제공
- `single { // definition }` : singleton bean 정의를 제공(`bean`으로 별칭)
- `get()` : 컴포넌트(구성요소) 종속성을 해결(이름, 범위 혹은 파라미터를 사용할 수 도 있음)
- `bind()` : 주어진 bean 정의에 바인딩을 할 타입 추가
- `binds()` : 주어진 bean 정의에 대한 타입 배열 추가
- `scope { // scope group }` : `scoped` 정의에 대한 논리적 그룹 정의
- `scoped { // definition }` : scope 내에서만 존재할 bean 정의를 제공

named() 함수를 사용하면 string이나 특정한 타입으로 한정자를 제공할 수 있다. 정의의 이름을 지정할 때 사용된다.

### Writing a module

Koin 모듈은 모든 구성요소를 선언하는 공간이다. `module` 함수를 이용해 Koin 모듈을 선언한다.

    val myModule = module {
       // your dependencies here
    }

위와 같은 모듈 함수에서 [Definitions](Definitions.md) 에 설명된 대로  컴포넌트를 선언할 수 있다.

### 이전

​     

### 다음

[Definitions](Definitions.md)