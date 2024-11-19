---
tags:
  - setting
---
# 기본 정보

- .editorconfig 가 formatter 보다 항상 우선 적용됩니다.
- SWAG 에서 배포되는 Coding Convention 은 editorconfig 를 기본으로 합니다.

> [!info]
> 경로로 링크를 대체합니다.
> \\99 Resources\\.editorconfig
> 


# 설정 방법

## 1. editorconfig

- IntelliJ 기준으로 프로젝트의 루트 폴더에 위치시키는 것으로 적용 가능합니다.
- .eidtorconfig 예시는 아래와 같습니다.

```
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true

[*.bat]
end_of_line = crlf

[*.java]
indent_style = tab
indent_size = 4
tab_width = 4
trim_trailing_whitespace = true

[*.log]
# [/logs/**]
end_of_line = unset

```


## 2. IntelliJ Formatter

- 설정 → 코드 스타일 → Java
- 외부에서 배포한 파일을 사용할 경우, xml 로 배포된 formatter 파일을 import 아래 예시에 따라 Import 하여 사용할 수 있습니다.

![IntelliJ Formatter](intellij_formatter_example.png)


# 참고

- 네이버 Java 코딩 컨벤션 : https://github.com/naver/hackday-conventions-java