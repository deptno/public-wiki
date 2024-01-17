# gradle
> 안드로이드 빌드 스크립트로 사용됨

## custom build type
```gradle
    buildTypes {
        dev {
            initWith(buildTypes.release)
            signingConfig signingConfigs.debug
    
            matchingFallbacks = ['release']
        }
    }
```
- release 설정으로 빌드를 하되 `signingConfig` 를 `debug` 등 다른 환경을 사용하고싶어서 생성
- 커스텀 빌드 타입인지라 다른 라이브러리에서 참조할때 없을 가능성이 높기 때문에 `matchingFallbacks` 에 대해서 빌드시에 라이브러리가 어느 환경을 참조할지 설정해줘야한다
  - 시스템을 잘 이해하진 못하지만 생성한 `dev` 빌드 타입이 라이브리에서 사용하고 있는 경우 위험할 수 있다고 추측
- 아래 명령으로 실행
```sh 
./gradlew assembleDev
```


##  error
### FAILURE: Build failed with an exception.
```sh 
FAILURE: Build failed with an exception.

* What went wrong:
Could not determine the dependencies of task ':app:lintVitalReportDev'.
> Could not resolve all task dependencies for configuration ':app:devRuntimeClasspath'.
   > Could not resolve com.facebook.react:react-android.
     Required by:
         project :app
      > No matching variant of com.facebook.react:react-android:0.73.2 was found. The consumer was configured to find a component for use during runtime, preferably optimized for Android, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'dev', attribute 'com.android.build.api.attributes.AgpVersionAttr' with value '8.1.1', attribute 'org.jetbrains.kotlin.platform.type' with value 'androidJvm' but:
          - Variant 'debugVariantDefaultApiPublication' capability com.facebook.react:react-android:0.73.2:
              - Incompatible because this component declares a component for use during compile-time, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'debug' and the consumer needed a component for use during runtime, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'dev'
              - Other compatible attributes:
                  - Doesn't say anything about com.android.build.api.attributes.AgpVersionAttr (required '8.1.1')
                  - Doesn't say anything about its target Java environment (preferred optimized for Android)
                  - Doesn't say anything about org.jetbrains.kotlin.platform.type (required 'androidJvm')
          - Variant 'debugVariantDefaultRuntimePublication' capability com.facebook.react:react-android:0.73.2 declares a component for use during runtime:
              - Incompatible because this component declares a component, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'debug' and the consumer needed a component, as well as attribute 'com.android.build.api.attributes.BuildTypeAttr' with value 'dev'
              - Other compatible attributes:
                  - Doesn't say anything about com.android.build.api.attributes.AgpVersionAttr (required '8.1.1')
                  - Doesn't say anything about its target Java environment (preferred optimized for Android)
                  - Doesn't say anything about org.jetbrains.kotlin.platform.type (required 'androidJvm')
```
- custom build build type 으로 `dev` 생성후 발생 [[#custom build type]] 참조
- 커스텀 빌드 타입인지라 다른 라이브러리에서 참조할때 없을 가능성이 높기 때문에 `matchingFallbacks` 에 대해서 빌드시에 라이브러리가 어느 환경을 참조할지 설정해줘야한다
- release 라이브러리를 테스트하고자하므로 release 로 지정해준다
```gradle
    buildTypes {
        dev {
            // ...
            matchingFallbacks = ['release']
        }
    }
```

## link
- [[android]]
