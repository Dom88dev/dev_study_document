# Fragment Factory

AndroidX가 Android Fragment 관련 기능 확장을 위한 androidx.fragment 패키지 제품군을 출시 한 이후에 대한 대응과 과련되어 있다.
[Android JetPack Fragment](https://developer.android.com/jetpack/androidx/releases/fragment)

# Fragment Factory

`1.2.0- *` 버전부터 `Fragment` 클래스의 인스턴스를 만드는 전용 클래스 인 `[FragmentFactory](https://developer.android.com/reference/kotlin/androidx/fragment/app/FragmentFactory)`가 도입되었습니다.
Koin은 `KoinFragmentFactory`를 가져와 `Fragment` 인스턴스를 직접 주입 할 수 있습니다.

# Setup Fragment Factory

시작시 KoinApplication 선언에서 `fragmentFactory()` 키워드를 사용하여 기본 `KoinFragmentFactory` 인스턴스를 설정하십시오.

    startKoin {
        androidLogger(Level.DEBUG)
        androidContext(this@MainApplication)
        androidFileProperties()
        // setup a KoinFragmentFactory instance
        fragmentFactory()
    
        modules(...)
    }

# Declare & Inject your Fragment

Fragment 인스턴스를 선언하려면 Koin 모듈에서 Fragment 인스턴스를 Fragment로 선언하고 생성자 주입을 사용하십시오.

    class MyFragment(val myService: MyService) : Fragment(){
    		...
    }
    
    val appModule = module {
        single { MyService() }
        fragment { MyFragment(get()) }
    }

# Get your Fragment

호스트 `Activity` 클래스에서 `setupKoinFragmentFactory()`를 사용하여 프래그먼트 팩토리를 설정하십시오.

    class MyActivity : AppCompatActivity() {
    
        override fun onCreate(savedInstanceState: Bundle?) {
            // Koin Fragment Factory
            setupKoinFragmentFactory()
    
            super.onCreate(savedInstanceState)
            //...
        }
    }

그런 후 `supportFragmentManager`로 `Fragment`를 검색하십시오.

    supportFragmentManager.beginTransaction()
                .replace(R.id.mvvm_frame, MyFragment::class.java, null, null)
                .commit()

# Fragment Factory & Koin Scopes

Koin Activity scope를 사용하려면 scope 내의 Fragment를 `scoped` 정의로 선언해야 합니다.

    val appModule = module {
        scope<MyActivity> {
            scoped { MyFragment(get()) }
        }
    }

그 후 해당 범위(scope)로 Koin Fragment Factory를 설정하십시오.

    class MyActivity : AppCompatActivity() {
    
        override fun onCreate(savedInstanceState: Bundle?) {
            // Koin Fragment Factory
            setupKoinFragmentFactory(currentScope)
    
            super.onCreate(savedInstanceState)
            //...
        }
    }

### **이전**

[ViewModel](ViewModel.md)

### **다음**