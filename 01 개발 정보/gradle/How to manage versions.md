---
tags:
  - gradle
  - setting
---
# 버전 관리 방법

## 요약

- 7.4 부터 toml 파일을 활용한 version catalog 방식을 지원합니다.
- [Sharing dependency versions between projects](https://docs.gradle.org/current/userguide/platforms.html)


## 설정 방법

- root project 하위에 gradle 폴더에 libs.versions.toml 파일을 생성합니다.
- 해당 파일에 toml 형식으로 version 을 작성합니다.


## 작성 방법

### 1. libs.versions.toml

- 각 category 별로 하위에 내용을 작성합니다.

#### 1) versions

- plugins 와 libraries 에서 정의 할 version 정보를 정의하는 구역입니다.
- 여기에 선언해서 정의하지 않고, 직접 값을 입력해도 상관은 없습니다.

#### 2) libraries

- dependencies 에 추가될 library 들의 정보를 정의하는 구역입니다.

#### 3) bundles

- libraries 에 선언한 관련 library 들을 bundle 로 묶어서 정의하는 용도로 사용합니다.

#### 4) plugins

- plugin 을 정의합니다.

#### 5) 예시

- versions 에 작성된 상수 parameter 는 plugins 나 libraries 의 version.ref 에서 사용됩니다.
- lombok 과 같이 상위 libarary package 에 의해 version dependecy 가 정해지는 경우는 기존과 동일하게 version 정보를 생략해도 상관 없습니다.

```toml
[versions]
spring-boot = "2.7.7"
springdoc = "1.6.13"

...

[plugins]
spring-boot = { id = "org.springframework.boot", version.ref = "spring-boot" }

...

[libraries]
spring-boot-starter = { group = "org.springframework.boot", name = "spring-boot-starter" }
springdoc = { group = "org.springdoc", name = "springdoc-openapi-ui", version.ref = "springdoc" }
lombok = { group = "org.projectlombok", name = "lombok" }

```

### 2. build.gradle.kts

- root project 에서는 libs.~~~ 로 접근, sub project 에서는 rootProject.libs.~~~ 로 접근합니다.
- 하이픈(-) 은 닷(.) 으로 치환해서 접근 가능합니다.
    - 즉, toml 에서 **spring-boot-starter** 로 정의됐다면 → kts 에서 **spring.boot.start** 로 접근하면 됩니다.
- IntelliJ 의 경우 build 에는 이상 없으나, 'libs' prefix 를 인식하지 못하는 문제가 발생할 수 있습니다.
    - 이 경우, **@Suppress("DSL_SCOPE_VIOLATION")** annotation 을 추가하면 해결 가능합니다.

![IntelliJ error](exam_dsl_scope_violation.png)

#### 1) 예시

```kotlin

@Suppress("DSL_SCOPE_VIOLATION")

plugins {
    alias(libs.plugins.spring.boot)
    ...
}

...

subprojects {
    val libraries = rootProject.libs

    dependencies {
        implementation(libraries.spring.boot.starter)
        implementation(rootProject.libs.lombok)
        ...
    }
    ...
}

```


## References

- Posting
    - [Gradle Version Catalog](https://brunch.co.kr/@oemilk/221)
    - [GRADLE VERSION CATALOGS FOR AN AWESOME DEPENDENCY MANAGEMENT](https://www.droidcon.com/2022/05/13/gradle-version-catalogs-for-an-awesome-dependency-management/)
    - [Good code red in buildSrc/build.gradle.kts when using "libs" from the new VersionCatalog support](https://youtrack.jetbrains.com/issue/KTIJ-19370)
- git
    - [build.gradle.kts](https://github.com/kotest/kotest-extensions-spring/blob/master/build.gradle.kts)
    - [libs.versions.toml](https://github.com/kotest/kotest-extensions-spring/blob/master/gradle/libs.versions.toml)
