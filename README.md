# 프로젝트 설정 가이드

## 1. Git 서브모듈 추가
Gradle 설정을 위한 서브모듈을 추가하려면 다음 명령어를 실행합니다:

```bash
git submodule add https://github.com/hanbong5938/gradle-config.git gradle/libs
git submodule update --init --recursive
```

## 2. `settings.gradle.kts` 설정
서브모듈에서 제공하는 `libs.versions.toml` 파일을 사용하기 위해 `settings.gradle.kts`에 다음 내용을 추가합니다:

```kotlin
dependencyResolutionManagement {
    versionCatalogs {
        create("libs") {
            from(files("gradle/libs/libs.versions.toml"))
        }
    }
}
```

## 3. 사용 예시
### 3.1 플러그인 적용
```kotlin
plugins {
    id("org.springframework.boot") version libs.versions.springBoot.get()
}
```

### 3.2 Java 버전 설정
```kotlin
java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(libs.versions.java.get().toInt())
    }
}
```

### 3.3 의존성 추가
```kotlin
dependencies {
    implementation("org.springframework.security:spring-security-rsa") {
        version { strictly(libs.versions.rsa.get()) }
    }
}
```

혹은 

```kotlin
dependencies {
   implementation(libs.spring.security.rsa)
}
```
 이렇게 설정할 수 있습니다


이제 프로젝트에서 `libs.versions.toml`에 정의된 버전을 활용하여 일관된 의존성 관리를 할 수 있습니다.

