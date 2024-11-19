---
tags:
  - gradle
  - jacoco
  - setting
---

# Jacoco 설정 방법

## 1. 요약

- gradle + kotlin 을 기준으로 작성됐습니다.


## 2. Plugin 설정

- gradle build 설정 파일에 jacoco plugin 을 추가하여, task 를 로딩합니다.

```kotlin
plugins {
    java
    jacoco
}
```

- IDE 의 gradle 설정 화면에서 verification 그룹에서 아래 2개의 task 확인이 가능합니다.
    - jacocoTestCoverageVerification
    - jacocoTestReport
- jacoco toolVersion 을 설정해 줍니다. 특별한 사유가 없으니 최신 버전으로 설정합니다. 최신 버전에 대한 정보는 [Configuring the JaCoCo Plugin](https://docs.gradle.org/current/userguide/jacoco_plugin.html#sec:configuring_the_jacoco_plugin)  을 참고합니다.

```kotlin
jacoco {
    toolVersion = "0.8.8"
}
```


## 3. Task 설정

### 1) jacocoTestReport

- 발행할 report 의 format 을 정의해줍니다.
- 아래 예시에서는 html 과 xml 로 report 를 생성하도록 정의했습니다.
- jacocoTestReport 가 생성된 후에 Coverage 확인을 할 수 있도록 finalizedBy 로 연동 시켜줍니다.

```kotlin
tasks.jacocoTestReport {
    reports {
        html.required.set(true)
        xml.required.set(true)
        csv.required.set(false)
    }
    finalizedBy(tasks.jacocoTestCoverageVerification)
}
```


### 2) jacocoTestCoverageVerification

- jacocoTestReport 결과를 바탕으로 확인할 code coverage 를 설정합니다.
- 아래 예시에서는 line coverage 100% 로 확인하겠다는 의미 입니다.
- 설정에 대한 내용은 [Enforcing code coverage metrics](https://docs.gradle.org/current/userguide/jacoco_plugin.html#sec:jacoco_report_violation_rules) 의 예시를 참고로 작성하면 됩니다.

```kotlin
tasks.jacocoTestCoverageVerification {
    violationRules {
        rule {
            element = "CLASS"
            limit {
                counter = "LINE"
                value = "COVEREDRATIO"
                minimum = "1.00".toBigDecimal()
            }
        }
    }
}
```

- violationRules 에서 제외하고 싶은 영역은 아래와 같은 형태로 exclude 처리하면 됩니다.

```kotlin
tasks.jacocoTestCoverageVerification {
    violationRules {
        ....
    }

    // listOf 도 가능 : val excludes = listOf("", ""...);
    val excludes = mutableListOf<String>()
    excludes.add("com/neurophet/neo/common/api/제외할클래스.class")
    classDirectories.setFrom(
        sourceSets.main.get().output.asFileTree.matching {
            exclude(excludes)  
        }
    )
}

```


### 3) build

- build 때 마다 jacoco report 를 발행하고 coverage 를 확인하고 싶을 경우, 'build' task 에 dependency 를 연결합니다.
- 아래와 같이 설정 시 'build' task 를 실행하면 dependsOn 으로 선언되어 있는 'jacocoTestReport' 가 먼저 수행됩니다.

```kotlin
tasks.build {
    dependsOn(tasks.jacocoTestReport)
}
```


## 4. References

- [Gradle - The JaCoCo Plugin](https://docs.gradle.org/current/userguide/jacoco_plugin.html)
- [JaCoCo Java Code Coverage Library](https://www.jacoco.org/jacoco/)
