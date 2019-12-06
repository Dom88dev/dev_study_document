# ViewModel

koin-android-viewmodel 프로젝트는 Android Architecture ViewModel 기능을 제공하기 위해 있다.

# ViewModel DSL

`koin-android-viewmodel`은 `single` 및 `factory`를 보완하는 새로운 `viewModel` DSL 키워드를 도입하여 ViewModel 구성 요소를 선언하고 Android Component lifecycle에 바인딩한다.

    val appModule = module {
    
        // ViewModel for Detail View
        viewModel { DetailViewModel(get(), get()) }
    
    }

선언된 컴포넌트는 `android.arch.lifecycle.ViewModel` 클래스를 상속받아야 한다. 
클래스의 생성자를 주입하고 `get()` 함수를 사용해 종속성을 주입할 수 있다.

`viewModel` 키워드는 뷰모델의 factory 인스턴스를 선언하는데 도움이 된다. 이 인스턴스는 내부 `ViewModelFactory`에 의해 처리되며 필요한 경우 ViewModel 인스턴스를 다시 연결한다.

`viewModel` 키워드는 주입 파라미터를 사용할 수 있게도 해준다. 

# Injecting your ViewModel

Activity, Fragment 또는 Service에 ViewModel을 주입하려면

- `by viewModel()` : lazy 하게  뷰모델 인스턴스를 프로퍼티에 주입
- `getViewModel()` : 곧장 뷰모델 인스턴스를 가져옴

    class DetailActivity : AppCompatActivity() {
    
        // Lazy inject ViewModel
        val detailViewModel: DetailViewModel by viewModel()
    }

비슷한 방식으로 Java에서 `ViewModelCompat`의 `viewModel()` 또는 `getViewModel` 정적 함수를 사용하면 ViewModel 인스턴스를 삽입 할 수 있다.

    // import viewModel
    import static org.koin.android.viewmodel.compat.ViewModelCompat.viewModel;
    
    // import getViewModel
    import static org.koin.android.viewmodel.compat.ViewModelCompat.getViewModel;
    
    public class JavaActivity extends AppCompatActivity {
    
        // lazy ViewModel
        private Lazy<DetailViewModel> viewModelLazy = viewModel(this, DetailViewModel.class);
    
        // directly get the ViewModel instance
        private DetailViewModel viewModel = getViewModel(this, DetailViewModel.class);
    
        @Override
        protected void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_simple);
    
            //...
        }
    }

# Shared ViewModel

Fragments와 해당 호스트 Activity간에 하나의 ViewModel 인스턴스를 공유 할 수 있다.
Fragment에 shared ViewModel을 주입하려면 다음의 것들을 사용하면 된다:

- by sharedViewModel() : lazy 하게  shared 뷰모델 인스턴스를 프로퍼티에 주입
- getSharedViewModel() : 곧장 shared 뷰모델 인스턴스를 가져옴

    /* ViewModel을 한번만 선언하면 된다. */
    val weatherAppModule = module {
    
        // WeatherViewModel declaration for Weather View components
        viewModel { WeatherViewModel(get(), get()) }
        /* 참고 : ViewModel의 한정자는 ViewModel의 태그로 처리된다. */
    }
    
    /* 선언 후 Activity와 Fragment에서 재사용하면 된다. */
    class WeatherActivity : AppCompatActivity() {
    
        /*
         * Koin으로 WeatherViewModel을 선언하고 의존성을 주입
         */
        private val weatherViewModel by viewModel<WeatherViewModel>()
    }
    
    class WeatherHeaderFragment : Fragment() {
    
        /*
         * WeatherActivity와 공유하는 WeatherViewModel을 선언
         */
        private val weatherViewModel by sharedViewModel<WeatherViewModel>()
    }
    
    class WeatherListFragment : Fragment() {
    
        /*
         * WeatherActivity와 공유하는 WeatherViewModel을 선언
         */
        private val weatherViewModel by sharedViewModel<WeatherViewModel>()
    }

ViewModel을 공유하는 Activity는 `viewModel()` 또는 `getViewModel()`로 ViewModel을 주입.
Fragment는 `sharedViewModel()`로 공유 ViewModel을 재사용.

Java Fragment의 경우 sharedViewModelCompat의 `sharedViewModel` 또는 `getSharedViewModel`을 사용해야 한다.

    // import sharedViewModel static function
    import static org.koin.android.viewmodel.compat.SharedViewModelCompat.sharedViewModel;
    
    public class JavaFragment extends Fragment {
    
        private Lazy<WeatherViewModel> sharedViewModelLazy = sharedViewModel(this, WeatherViewModel.class);
    		
    		private WeatherViewModel sharedViewModel = getSharedViewModel(this, WeatherViewModel.class);
    
        @Override
        public void onViewCreated(@NonNull View view, @Nullable Bundle savedInstanceState) {
            super.onViewCreated(view, savedInstanceState);
            //...
        }
    }

# ViewModel and injection parameters

viewModel 키워드 및 injection API는 주입 매개 변수와 호환된다.

    /* 모듈에서 */
    val appModule = module {
    
        // ViewModel for Detail View with id as parameter injection
        viewModel { (id : String) -> DetailViewModel(id, get(), get()) }
    }
    
    /* 주입 호출 시 */
    class DetailActivity : AppCompatActivity() {
    
        val id : String // id of the view
    
        // Lazy inject ViewModel with id parameter
        val detailViewModel: DetailViewModel by viewModel{ parametersOf(id)}
    }

# ViewModel and State Bundle

`koin-androidx-viewmodel : 2.1.0-alpha-3`에서 ViewModel의 상태 번들을 처리하는 더 깨끗한 방법을 검토되었다.
방법은 다음과 같다.
Bundle 객체를 ViewModel의 초기화 상태로 주입 매개 변수로 전달하라.
생성자에 새로운 속성 유형 SavedStateHandle을 추가하라.

    class MyStateVM(val handle: SavedStateHandle, val myService : MyService) : ViewModel()

Koin 모듈에서 파라미터 주입을 사용하라.

    viewModel { (handle: SavedStateHandle) -> MyStateVM(handle, get()) }

Activity / Fragment에서 기본 상태를 매개 변수로 전달하라.

    val myStateVM: MyStateVM by viewModel { parametersOf(Bundle(), "vm1") }

### **이전**

[Koin Scope](Koin%20Scope.md)

### **다음**

[Fragment Factory](Fragment%20Factory.md)