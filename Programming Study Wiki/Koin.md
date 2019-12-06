# Koin

This template documents What is Koin and How to use it in Korean.(for Android)
references from [KoinDocuments](https://doc.insert-koin.io/#/)

# What is KOIN?

Kotlin 개발자를 위한 실용적인 경량 DI(Dependency Injection) 프레임 워크다.

프록시, 코드 생성, 리플렉션 없이 기능적 해결법만 사용하여 순수한 Kotlin으로 작성되었다.

Koin은 [`DSL`](DSL%20Domain%20Specific%20Language.md)로 가벼운 컨테이너며 실용적인 API다. 

# Setup Koin for Your project

프로젝트에 Koin 적용 방법.

### Current Version

현재(02/12/19) Koin 최신 버전 정보.

    // Current stable version
    koin_version = "2.0.1"
    // Latest unstable version
    koin_version = "2.1.0-alpha-4"

### Gradle dependencies

Koin을 추가하기 위해 다음과 같은 Gradle dependencies를 프로젝트에 추가.

Koin packages 는 JCenter에 발행되어 있다.

    // Add Jcenter to your repositories if needed
    // Project level의 gradle 파일에 추가
    repositories {
    	jcenter()
    }


​    
    // App level의 gradle 파일에 dependencies 추가
    
    // Koin for Android
    compile "org.koin:koin-android:$koin_version"
    
    // Koin AndroidX Scope feature
    compile "org.koin:koin-androidx-scope:$koin_version"
    
    // Koin AndroidX ViewModel feature
    compile "org.koin:koin-androidx-viewmodel:$koin_version"
    
    // Koin AndroidX Fragment Factory (unstable version)
    compile "org.koin:koin-androidx-fragment:$koin_version"

# Koin Core

[Koin DSL](Koin/Koin%20DSL.md)

[Definitions](Koin/Definitions.md)

[Modules](Koin/Modules.md)

# Koin Android

[Start on Android](Koin/Start%20on%20Android.md)

[Android DSL](Koin/Android%20DSL.md)

[Retrieve Instances](Koin/Retrieve%20Instances.md)

[Koin Scope](Koin/Koin%20Scope.md)

[ViewModel](Koin/ViewModel.md)

[Fragment Factory](Koin/Fragment%20Factory.md)

# Github

[InsertKoinIO](https://github.com/InsertKoinIO/koin)
