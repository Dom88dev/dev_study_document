# Modules

Koin을 사용해 모듈안의 정의를 설명한다. 이 섹션에서는 모듈을 선언, 구성 및 링크하는 방법을 살펴 본다.

# What is a module?

Koin 모듈은 Koin 정의를 수집하기 위한 “공간”이다. `module` 함수로 선언되었다.

    val myModule = module {
        // Your definitions ...
    }

# Using several modules

컴포넌트가 반드시 동일한 모듈에 있을 필요는 없다. 모듈은 정의를 구성하는 데 도움이 되는 논리적 공간이며 다른 모듈의 정의에 따라 달라질 수 있다. 정의가 지연된 다음 컴포넌트가 요청한 경우에만 해결된다.

별도의 모듈에 링크 된 컴포넌트가 있는 예를 들어 보겠다.

    // ComponentB <- ComponentA
    class ComponentA()
    class ComponentB(val componentA : ComponentA)
    
    val moduleA = module {
        // Singleton ComponentA
        single { ComponentA() }
    }
    
    val moduleB = module {
        // Singleton ComponentB with linked instance ComponentA
        single { ComponentB(get()) }
    }

Koin에는 import 개념이 없다. Koin 정의가 지연된다 (Koin 정의가 Koin 컨테이너로 시작되었지만 인스턴스화 되지 않는다.) 인스턴스는 해당 유형에 대한 요청에 의해서만 작성된다.

Koin 컨테이너를 시작할 때 사용한 모듈 목록을 선언해야 한다.

    // Start Koin with moduleA & moduleB
    startKoin{
        modules(moduleA,moduleB)
    }

그런 다음 Koin은 주어진 모든 모듈의 종속성을 해결한다.

# Linking modules strategies

모듈 간의 정의가 게으르기(lazy) 때문에 모듈을 사용하여 다른 전략 구현을 구현할 수 있다. 모듈 당 구현을 선언하시오.

레포지토리(저장소) 및 데이터 소스를 예로 들어 보겠다. 저장소에는 데이터 소스가 필요하며 데이터 소스는 로컬 또는 원격의 두 가지 방법으로 구현할 수 있다.

    class Repository(val datasource : Datasource)
    interface Datasource
    class LocalDatasource() : Datasource
    class RemoteDatasource() : Datasource
    
    /* 우리는 이러한 컴포넌트를 3 가지 모듈로 선언 할 수 있다 (저장소 및 데이터 소스 구현 당 하나씩) */
    val repositoryModule = module {
        single { Repository(get()) }
    }
    
    val localDatasourceModule = module {
        single<Datasource> { LocalDatasource() }
    }
    
    val remoteDatasourceModule = module {
        single<Datasource> { RemoteDatasource() }
    }
    
    /* 그런 다음 올바른 모듈 조합으로 Koin을 시작하면 된다. */
    // Load Repository + Local Datasource definitions
    startKoin {
        modules(repositoryModule,localDatasourceModule)
    }
    
    // Load Repository + Remote Datasource definitions
    startKoin {
        modules(repositoryModule,remoteDatasourceModule)
    }

# Overriding definition or module

Koin에서는 기존 정의 (유형, 이름, 경로 ...)를 재정의 할 수 없다. 시도하면 오류가 발생한다.

    val myModuleA = module {
    
        single<Service> { ServiceImp() }
    }
    
    val myModuleB = module {
    
        single<Service> { TestServiceImp() }
    }
    
    // Will throw an BeanOverrideException
    startKoin {
        modules(myModuleA,myModuleB)
    }
    
    /* 정의의 재정의를 허용하려면 override 매개 변수를 사용해야 한다. */
    val myModuleA = module {
    
        single<Service> { ServiceImp() }
    }
    
    val myModuleB = module {
    
        // override for this definition
        single<Service>(override#true) { TestServiceImp() }
    }
    /* or */
    val myModuleA = module {
    
        single<Service> { ServiceImp() }
    }
    
    // Allow override for all definitions from module
    val myModuleB = module(override#true) {
    
        single<Service> { TestServiceImp() }
    }

모듈을 나열하고 정의를 재정의 할 때 순서가 중요하다. 모듈 목록의 마지막에 재정의 정의가 있어야 한다.

### **이전**

[Definitions](Definitions.md)

### **다음**

[Start on Android](Start%20on%20Android.md)