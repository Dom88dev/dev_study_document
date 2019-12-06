# Retrieve Instances

일부 모듈을 선언하고 Koin을 시작하면 Android Activity, Fragment 또는 Service에서 인스턴스를 어떻게 가져올 수 있을까?

# Activity, Fragment & Service as KoinComponents

Activity, Fragment 및 Service는 KoinComponents extension으로 확장되어 다음에 액세스 할 수 있다.

- `by inject()` : Koin 컨테이너로 부터 lazy하게(사용시점에) 인스턴스를 가져온다.
- `get()` : Koin 컨테이너로 부터 즉각적으로 인스턴스를 가져온다.
- `release()` : 경로에서 모듈의 인스턴스를 해제한다.
- `getProperty()` / `setProperty()` : 속성값을 가져오거나 설정한다.

    /* 'Presenter' 컴포넌트를 선언한 모듈의 경우 */
    val androidModule = module {
        // a factory of Presenter
        factory { Presenter() }
    }
    
    /* 우린 다음처럼 lazy한 주입으로 프로퍼티를 선언할 수 있다. */
    class DetailActivity : AppCompatActivity() {
    
        // Lazy injected Presenter instance
        override val presenter : Presenter by inject()
        
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            // ...
        }
    }
    
    /* 또는 다음과 같이 곧장 인스턴스를 가져올 수 있다. */
    class DetailActivity : AppCompatActivity() {
    
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
        
            // Retrieve a Presenter instance
            val presenter : Presenter = get()
        }
    }
    

### Need inject() and get() anywhere else?

다른 클래스에서 인스턴스를 `inject()` 또는 `get()`해야 하는 경우 KoinComponent 인터페이스로 태그를 지정하시오.

### **이전**

[Android DSL](Android%20DSL.md)

### **다음**

[Koin Scope](Koin%20Scope.md)