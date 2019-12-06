# Start on Android

안드로이드에서 Koin 사용법을 알아본다.

# startKoin() from your Application

Application 클래스에서 `startKoin` 함수를 사용하고 다음과 같이 `androidContext`를 사용하여 안드로이드의 컨텍스트를 삽입 할 수 있다.

    class MainApplication : Application() {
    
        override fun onCreate() {
            super.onCreate()
    
            startKoin {
                // Koin Android logger
                androidLogger()
                //inject Android context
                androidContext(this@MainApplication)
                // use modules
                modules(myAppModules)
            }
    
        }
    }

# Starting Koin with Android context from elsewhere?

다른 Android 클래스에서 Koin을 시작해야 하는 경우 `startKoin` 함수를 사용하여 다음과 같이 Android Context 인스턴스를 제공 할 수 있다.

    startKoin {
        //inject Android context
        androidContext(/* your android context */)
        // use modules
        modules(myAppModules)
    }

# Koin Logging

KoinApplication 인스턴스 내에 `AndroidLogger()`[Koin 로거의 Android 구현]를 사용하는 extension androidLogger가 있다.

필요에 따라 이 로거를 변경하면 된다. \ Koin Logger 는 X

    class MainApplication : Application() {
    
        override fun onCreate() {
            super.onCreate()
    
            startKoin {
                //inject Android context
                androidContext(/* your androic context */)
                // use Android logger - Level.INFO by default
                androidLogger()
                // use modules
                modules(myAppModules)
            }
        }
    }

# Properties

`assets / koin.properties` 파일에서 Koin 속성을 사용하여 키 / 값을 저장할 수 있다.

    // Shut off Koin Logger
    class MainApplication : Application() {
    
        override fun onCreate() {
            super.onCreate()
    
            startKoin {
                //inject Android context
                androidContext(/* your androic context */)
                // use Android logger - Level.INFO by default
                androidLogger()
                // use properties from assets/koin.properties
                androidFileProperties()
                // use modules
                modules(myAppModules)
            }
        }
    }

### **이전**

[Modules](Modules.md)

### **다음**

[Android DSL](Android%20DSL.md)