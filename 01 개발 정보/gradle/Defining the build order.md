---
tags:
  - gradle
  - setting
---
# 빌드 순서 정의

## 외부 프로젝트의 빌드 결과를 이용해 빌드가 필요한 경우

### 1. 문제 상황

- multi module 구조에서 dependency 가 존재할 경우
- 일반적인 구조의 경우에는 dependency 를 아래와 같이 정의하기 때문에 크게 문제가 없지만,

```kotlin
dependencies {
    implementation(project(":module-common"))
}
```

- 만약 아래와 같이 다른 모듈의 build 에 의해 생성된 library 파일을 직접 implementation 하는 경우에는 문제가 발생할 수 있습니다.

```kotlin
dependencies {
    implementation(files("$rootDir/libs/module-analyzer-1.0.0.jar"))
}
```


### 2. 해결 방법

- build 전에 참조하고 있는 module 이 먼저 build 되어 library 파일이 생성되어야 하므로, dependsOn 을 선언하여 순차적으로 처리되도록 정의해야 합니다.

```kotlin
tasks.compileJava {
    dependsOn(":module-analyzer:build")
}
```


### 3. 추가 정보

- 전체 project 에 대한 build 수행 시, multi module 들이 각각 async 하게 동시에 build 되는 것으로 확인됩니다. ( 혹은, build 시작시 모든 task 들이 동시에 configuration 되는 것으로 보이는데, 이것 때문에 문제가 발생할 수도 있는 것 같음.. )
- 이때는 dependency 를 물고 순차적으로 build 가 될 수 있도록 "build" 대신 **"buildDependents"** 사용합니다.
- 경우에 따라서는 아래와 같이 build task 를 생성해서 사용할 수도 있습니다.

```kotlin
task("buildInOrder") {
    group = "build"
    dependsOn("clean")
    dependsOn(":app:build")
}
```



## 외부 프로젝트의 빌드 결과를 이용해 폴더 구성이 필요한 경우

### 1. 문제 상황

- build 완료 후, 'libs' 폴더 하위에 'plugins' 폴더를 생성하고,
- 해당 폴더에 product 하위에 빌드된 관련 plugin 들을 복사해서 넣어야 하는 경우, 아래와 같은 copy task 를 생성할 수 있습니다.

```kotlin
task<Copy>("copyToLibsUnderAnalysis") {
    from("$rootDir/modules/components/product")
    include("product*.jar")
    into("$buildDir/libs/plugins")
    includeEmptyDirs = false
}

tasks.build {
    dependsOn(":modules:components:product:build")
    finalizedBy("copyToLibsUnderAnalysis")
}
```

- 단, 이 경우 아래와 같이 폴더구조 역시 그대로 가져오기 때문에 원하는 결과를 얻을 수 없을 수 있습니다.

![all copy](copy_all_plugins_folder.png)

### 2. 해결 방법

- FileCollection 을 이용해서 rename 하거나, lazy 및 doLast 등등의 방법을 다 시도해봤으나 잘 동작하지 않았습니다.
- 아래와 같이 eachFile 을 사용하여 대상 파일들의 정보를 수정해서 복사하도록 하면 동작합니다.
- 아래 예시는 path 를 name 으로 치환했습니다.

```kotlin
task<Copy>("copyToLibsUnderAnalysis") {
    from("$rootDir/modules/components/product") {
        include("**/build/libs/*.jar")
        eachFile { path = name }
    }
    into("$buildDir/libs/plugins")
    includeEmptyDirs = false
}
```