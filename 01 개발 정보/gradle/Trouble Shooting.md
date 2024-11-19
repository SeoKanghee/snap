---
tags:
  - gradle
  - trouble
---
# Trouble Shooting

## 1. Test Executor 1 fnished ~~~

- 90% 이상 확률로 gradle path 에 한글로 경로가 지정되어 있을 가능성이 큽니다.
    - IntelliJ 설정 경로 : "파일" → "설정" → "빌드, 실행, 배포" → "빌드 도구" → "Gradle" → "Gradle 사용자 홈"
- 해당 경로에 한글이 포함되지 않도록 다른 곳으로 옮겨줍니다.

![IntelliJ Gradle User Home](intellij_gradle_user_home_20230223.png)


## 2. 개별 Test 오류

- ItelliJ 의 gradle 경로를 한글이 없는 경로로 변경해도 run test 시 intelliJ 를 사용하도록 하면 동일한 문제가 발생합니다.
    - IntelliJ 설정 경로 : "파일" → "설정" → "빌드, 실행, 배포" → "빌드 도구" → "Gradle" → "Run tests using"

![IntelliJ Gradle Run tests using](intellij_gradle_run_test_using_20230223.png)

- 해당 값을 "IntelliJ IDEA" 에서 "gradle" 로 변경하면 문제는 발생하지 않지만 아래와 같은 오류가 표시될 수 있습니다.
    - 오류 메시지 : org.junit.platform.launcher.core.EngineDiscoveryOrchestrator lambda$logTestDescriptorExclusionReasons$7


> [!warning]
>
> 아래 내용은 위의 오류 메시지가 거슬릴 경우 시도해볼만 방법입니다.
> 단, 시도했다가 한번 꼬이면 재설치로도 해결안되니, 거슬리는 채로 두는 것이 바람직할 것 같습니다.
> 

- IntelliJ 의 설정 정보가 일반적으로 Users/<사용자이름>/AppData/Roaming 하위에 들어가기 때문이라 설정 파일 저장 위치도 수정이 필요합니다.
- 경로 및 파일 이름 : <IntelliJ 설치 경로>/bin/**idea.properties**

```properties
#---------------------------------------------------------------------
# Uncomment this option if you want to customize a path to the settings directory.
#---------------------------------------------------------------------
# idea.config.path=${user.home}/.IdeaIC/config

#---------------------------------------------------------------------
# Uncomment this option if you want to customize a path to the caches directory.
#---------------------------------------------------------------------
# idea.system.path=${user.home}/.IdeaIC/system

#---------------------------------------------------------------------
# Uncomment this option if you want to customize a path to the user-installed plugins directory.
#---------------------------------------------------------------------
# idea.plugins.path=${idea.config.path}/plugins

#---------------------------------------------------------------------
# Uncomment this option if you want to customize a path to the logs directory.
#---------------------------------------------------------------------
# idea.log.path=${idea.system.path}/log
```

- 최초 설정은 위와 같이 주석 처리되어 있으므로 주석을 풀고, 일반적으로 한글 경로 문제를 발생시키는 {user.home} 이 포함된 경로를 다른 곳으로 수정해줍니다.

```properties
idea.config.path=<한글 없는 경로>/config
idea.system.path=<한글 없는 경로>/system
idea.plugins.path=${idea.config.path}/plugins
idea.log.path=${idea.system.path}/log
```
