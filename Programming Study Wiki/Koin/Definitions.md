# Definitions

Koin을 사용해 모듈에서의 정의를 설명한다. 이번 섹션에서는 모듈을 선언, 구성 및 링크하는 방법을 살펴 본다.

# Writing a module

Koin 모듈은 모든 구성요소를 선언하는 공간이다. `module` 함수를 이용해 Koin 모듈을 선언한다.

    val myModule = module {
       // your dependencies here
    }

위와 같은 모듈 함수에서 아래에 설명된 대로 컴포넌트를 선언할 수 있다.

# Defining a singleton

싱글톤 컴포넌트를 선언한다는 것은 Koin 컨테이너가 선언된 컴포넌트의 고유한 인스턴스를 유지한다는 것.

싱글톤 선언을 위해 모듈에서 `single` 함수를 사용

    class MyService()
    
    val myModule = module {
    
        // declare single instance for MyService class
        single { MyService() }
    }

# Defining your component within a lambda

single, factory, scoped 키워드를 사용하면 람다식을 이용해 컴포넌트를 선언할 수 있다. 이 람다식은 구성요소를 작성하는 방법에 대해 나타낸다. 일반적으로 생성자를 이용해 구성요소를 인스턴스화하지만 다른 표현식을 이용 할 수 도 있다.

`single { Class constructor // Kotlin expression }`

람다의 결과 타입은 컴포넌트의 기본 타입이다.

# Defining a factory

팩토리 컴포넌트 선언은 이 정의를 요청할 때마다 새 인스턴스를 제공하는 정의
(이 인스턴스는 나중에 다른 정의에 주입되지 않으므로 Koin 컨테이너에 의해 재교육[재사용?]되지 않는다.)

컴포넌트 빌드를 위해 람다식과 함께 `factory` 함수를 사용.

    class Controller()
    
    val myModule = module {
    
        // declare factory instance for Controller class
        factory { Controller() }
    }

Koin 컨테이너는 정의가 요청 될 때마다 새 인스턴스를 제공하므로 팩토리 인스턴스를 유지하지 않는다.

# Resolving & injecting dependecies

이제 인스턴스와 의존성 주입을 연결하려 한다. 
Koin 모듈에서 인스턴스를 해결하려면 `get()` 함수를 사용해 요청 된 필수 컴포넌트 인스턴스에 연결한다.
`get()` 함수는 일반적으로 생성자 값을 주입하기 위해 생성자에 사용된다.

Koin 컨테이너로 의존성 주입을하려면 생성자 주입 스타일로 작성해야 한다.
(클래스 생성자의 종속성을 해결) 
이렇게 하면 인스턴스가 Koin에서 주입 된 인스턴스로 생성된다.

아래에 몇가지 클래스로 예를 들었다.

    // Presenter <- Service
    class Service()
    class Controller(val view : View)
    
    val myModule = module {
    
        // declare Service as single instance
        single { Service() }
        // declare Controller as single instance, resolving View instance with get()
        single { Controller(get()) }
    }

# Definition: binding an interface

single 이나 factory 정의는 주어진 람다식 정의에서 나온 타입을 사용한다. 
(i.e] `single { T }` 정의에 부합하는 타입은 오직 표현식(람다)의 타입과 부합해야 한다.)

클래스와 구현된 인터페이스로 예를 들어 보겠다.

    // Service interface
    interface Service{
    
        fun doSomething()
    }
    
    // Service Implementation
    class ServiceImp() : Service {
    
        fun doSomething() { ... }
    }
    
    /* Koin에서는 다음과 같이 Kotlin의 as 캐스팅 연산자를 사용할 수 있다. */
    val myModule = module {
    
        // Will match type ServiceImp only
        single { ServiceImp() }
    
        // Will match type Service only
        single { ServiceImp() as Service }
    
    }
    
    /* 또한, <>을 이용한 유추 타입도 사용 가능하다. */
    val myModule = module {
    
        // Will match type ServiceImp only
        single { ServiceImp() }
    
        // Will match type Service only
        single<Service> { ServiceImp() }
    
    }

두번째 선언 스타일이 선호되며 이 문서의 이후 다른 부분에서 사용되어진다.

# Additional type binding

경우에 따라 하나의 정의에 여러 타입을 매칭시키길 원할 때가 있다.

클래스와 인터페이스로 예를 들어 보겠다.

    // Service interface
    interface Service{
    
        fun doSomething()
    }
    
    // Service Implementation
    class ServiceImp() : Service{
    
        fun doSomething() { ... }
    }
    
    /* 한 정의가 추가적인 타입을 바인드하게 하려면, bind 연산자를 클래스와 함께 사용하면 된다. */
    val myModule = module {
    
        // Will match types ServiceImp & Service
        single { ServiceImp() } bind Service::class
    }

여기서 `get()`을 사용하여 `Service` 타입을 직접 확인할 수 있다. 하지만 여러 정의가 `Service` 에 바인딩되어 있을 경우, `bind<>()` 함수를 사용해야만 한다.

# Definition: naming & default bindings

동일한 타입의 두 정의를 구별할 수 있도록 정의에 이름을 지정할 수 있다.
그저 정의와 이름을 함께 요청하면 된다.

    val myModule = module {
        single<Service>(named("default")) { ServiceImpl() }
        single<Service>(named("test")) { ServiceImpl() }
    }
    
    val service : Service by inject(name = named("default"))

필요한 경우 `get()` 및 `inject()` 함수를 통해 정의 이름을 지정할 수 있다. 
이 이름은 `named()` 함수에 의해 생성된 한정자다.

타입이 이미 정의에 바인딩 된 경우 기본적으로 Koin은 그 정의의 타입이나 이름으로 정의를 바인딩한다.

    val myModule = module {
        single<Service> { ServiceImpl1() }
        single<Service>(named("test")) { ServiceImpl2() }
    }
    
    val service1 : Service by inject() // ServiceImpl1 정의를 주입
    val service2 : Service by inject(named("test")) // ServiceImpl2 정의를 주입

# Declaring injection parameters

single, factory, scoped 정의에서 injection parameters(주입되어 정의에서 사용되어지는 파라미터)를 사용할 수 있다.

    class Presenter(val view : View)
    
    val myModule = module {
        single{ (view : View) -> Presenter(view) }
    }
    
    /* 해결된 의존성(get()으로 해결)과 반대로 인젝션 파라미터는 resolution API를 통해 전달된다.
    	 이는 인젝션 파라미터가 parametersOf 함수와 함께 get() 과 inject() 함수에 전달된 값임을 의미. */
    val presenter : Presenter by inject { parametersOf(view) }

# Using definition flags

Koin DSL은 몇가지 플래그들도 제공한다.

### Create instances at start

정의 또는 모듈은 createOnStart로 플래그가 지정되어 시작 시(또는 원할 때) 생성 될 수 있다.
먼저 모듈 또는 정의에서 createOnStart 플래그를 설정한다.

    /* CreateAtStart flag on a definition */
    val myModuleA = module {
    
        single<Service> { ServiceImp() }
    }
    
    val myModuleB = module {
    
        // eager creation for this definition
        single<Service>(createAtStart=true) { TestServiceImp() }
    }
    
    /* CreateAtStart flag on a module */
    val myModuleA = module {
    
        single<Service> { ServiceImp() }
    }
    
    val myModuleB = module(createAtStart=true) {
    
        single<Service>{ TestServiceImp() }
    }
    
    /* startKoin 함수는 자동으로 createAtStart 플래그가 지정된 정의 인스턴스를 생성한다. */
    // Start Koin modules
    startKoin {
        modules(myModuleA,myModuleB)
    }

특정한 시간에 정의를 로드하려는 경우(예 : UI 대신 백그라운드 쓰레드에서) 원하는 구성요소를 get하거나 inject 한다.

### Dealing with generics

Koin 정의는 제네릭 형식 인수를 고려하지 않다. 
예를 들어 아래 모듈은 List의 두 가지 정의를 정의하려고 시도한다.

    module {
        single { ArrayList<Int>() }
        single { ArrayList<String>() }
    }
    
    /* Koin은 이러한 정의로 시작하지 않으며, 정의를 다른 정의로 재정의하려는 것으로 이해한다.
    
    이를 허용하려면 이름 또는 위치 (모듈)를 통해 구별해야 할 두 가지 정의를 사용하라. 예를 들면 아래와 같다. */
    module {
        single(named("Ints")) { ArrayList<Int>() }
        single(named("Strings")) { ArrayList<String>() }
    }

### **이전**

[Koin DSL](Koin%20DSL.md)

### **다음**

[Modules](Modules.md)