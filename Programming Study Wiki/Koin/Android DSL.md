# Android DSL

다음은 Koin DSL에 추가 된 키워드다.

# Getting Android context inside a Module

`androidContext ()` & `androidApplication ()` 함수를 사용하면 Koin 모듈에서 컨텍스트 인스턴스를 가져 와서 Application 인스턴스가 필요한 표현식을 간단히 작성할 수 있다.

    val appModule = module {
    
        // create a Presenter instance with injection of R.string.mystring resources from Android
        factory {
            MyPresenter(androidContext().resources.getString(R.string.mystring))
        }
    }

### **이전**

[Start on Android](Start%20on%20Android.md)

### **다음**

[Retrieve Instances](Retrieve%20Instances.md)