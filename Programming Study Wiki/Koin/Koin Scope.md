# Koin Scope

koin-android-scope 프로젝트는 기존 Scope API에 Android scope 기능을 제공하기 위해 있다.

# Taming the Android lifecycle

Android 구성 요소는 주로 수명주기에 따라 관리된다.(Activity나 Fragment를 직접 인스턴스화 할 수 없다)
시스템은 모든 생성 및 관리를 수행하고 메소드들(onCreate, onStart…)에 대한 콜백을 수행한다.

Koin 모듈에서 Activity / Fragment / Service를 표현|접근할 수 없는 이유다. 속성에 종속성을 주입할 때 수명주기를 존중해야 한다.(UI 파트와 관련된 컴포넌트는 더 이상 필요하지 않은 즉시 릴리스해야한다)

그럼 다음의 컴포넌트들을

- long live components (Services, Data Repository …) : 여러 화면에서 사용되며 절대 삭제하지 않는다
- medium live components (user sessions …) : 여러 화면에서 사용되며 일정 시간 후 삭제해야 한다
- short live components (views) : 하나의 화면에서 사용되며 화면이 끝날 때 삭제해야 한다

생명이 긴 컴포넌트들은 single 정의로 쉽게 나타낼 수 있다. 중간 및 짧은 생면의 컴포넌트들은 몇 가지의 접근 방식이 있다.

MVP 아키텍처 스타일의 경우 Presenter는 UI를 지원하는 short live 컴포넌트다. presenter는 화면이 표시될 때마다 작성되어야 하고 그 화면이 사라지면 삭제해야 한다.

    /* 매번 새로운 Presenter가 생성된다 */
    class DetailActivity : AppCompatActivity() {
    
        // injected Presenter
        override val presenter : Presenter by inject()
    
    /*=============================================================================*/
    
    /* 모듈에선 다음과 같이 나타낼 수 있다 */
    
    /* factory : by inject () 또는 get ()이 호출 될 때마다 새 인스턴스를 생성 */
    val androidModule = module {
    
        // Factory instance of Presenter
        factory { Presenter() }
    }
    
    /* scope : 범위에 연결된 인스턴스를 생성 */
    val androidModule = module {
    
        scope(named("scope_id")) {
            scoped { Presenter() }
        }
    }

대부분의 Android 메모리 누수는 Android 이외의 컴포넌트에서 UI / Android 구성 요소를 참조하여 발생한다. 시스템은 참조를 유지하며 갈비지 콜렉션을 통해 참조를 완전히 삭제할 수 없다.

# CurrentScope - a scope tied to your lifecycle

Koin은 이미 Android 구성요소 수명주기에 바인딩된 currentScope 속성을 제공한다. 수명주기가 끝나면 해당 속성은 자동으로 닫힌다.
currentScope를 사용하려면 Activity에 대한 범위를 선언해야 만 한다.

    /* Activity 타입에 named() 한정자를 사용하는 방법은 다음과 같다 */
    val androidModule = module {
    
        scope(named<MyActivity>()) {
            scoped { Presenter() }
        }
    }
    
    class MyActivity : AppCompatActivity() {
    
        // inject Presenter instance from current scope
        val presenter : Presenter by currentScope.inject()

# Sharing instances between components with scopes

보다 확장 된 사용법으로 컴포넌트들간을 건너서 Scope 인스턴스를 사용할 수 있다. 예를 들어 UserSession 인스턴스를 공유해야 하는 경우가 있다.

    /* 먼저 scope 정의를 선언한다. */
    module {
        // Shared user session data
        scope(named("session")) {
            scoped { UserSession() }
        }
    }
    
    /* UserSession 인스턴스 사용을 시작해야 할 경우 범위를 작성한다. */
    val ourSession = getKoin().createScope("ourSession",named("session"))
    
    /* 그런 다음 필요한 곳 ​​어디에서나 사용하면 된다. */
    class MyActivity1 : AppCompatActivity() {
    
        val userSession : UserSession by ourSession.inject()
    }
    class MyActivity2 : AppCompatActivity() {
    
        val userSession : UserSession by ourSession.inject()
    }
    
    /* 또는 Presenter가 필요한 경우 Koin DSL로 주입할 수 도 있다. */
    class Presenter(val userSession : UserSession)
    /* 올바른 범위 id를 사용하여 생성자에 주입한다. */
    module {
        // Shared user session data
        scope(named("session")) {
            scoped { UserSession() }
        }
    
        // Inject UserSession instance from "session" Scope
        factory { (scopeId : ScopeID) -> Presenter(getScope(scopeId).get())}
    }
    
    /* scope를 끝내야할 때 scope를 닫으면 된다. */
    val ourSession = getKoin().getScope("ourSession")
    ourSession.close()

### **이전**

[Retrieve Instances](Retrieve%20Instances.md)

### **다음**

[ViewModel](ViewModel.md)