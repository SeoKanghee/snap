---
tags:
  - setting
  - docker
  - vdisk
---

# VDISK 관리

>[!NOTE]
> 기본 wsl 명령 : [https://learn.microsoft.com/ko-kr/windows/wsl/basic-commands](https://learn.microsoft.com/ko-kr/windows/wsl/basic-commands)
> 
> powsershell 은 관리자 권한으로 실행해서 작업해야 합니다.

## 1. VDISK 용량 줄이기

- wsl 이 모두 종료됐는지 확인합니다.
```shell
wsl -l -v

  NAME                   STATE           VERSION
* Ubuntu-20.04           Stopped         2
  docker-desktop         Stopped         2
  docker-desktop-data    Running         2
```

- 만약 Running 상태가 존재한다면 --shutdown 을 사용하여 종료합니다.
```shell
wsl --shutdown
```

- powershell 에서 diskpart.exe 를 실행합니다.
```shell
diskpart.exe

Microsoft DiskPart version 10.0.22621.1

Copyright (C) Microsoft Corporation.
On computer: DESKTOP-DFSIRFB

DISKPART>
```

- 용량을 줄일 vdisk 를 선택합니다.
 
>[!NOTE]
> 참고로 vhdx 파일 위치는 일반적으로 <span style="background-color:#fff5b1">"C:/Users/{사용자 계정}/AppData/Local/Docker/wsl/data"</span> 입니다.
> 
> 탐색기로 따라 들어가 보면 파일을 확인할 수 있습니다.

```shell
select vdisk file="C:\Users\{사용자 계정}\AppData\Local\Docker\wsl\data\ext4.vhdx"
```

- compact 명령을 실행합니다.
```shell
compact vdisk
```

![vdisk compact](vdisk_compact_20231121.png)


## 2. 저장소 옮기기

>[!WARNING]
> SSD 에서 HDD 로 옮기면 docker 가 많이 느려집니다.

- 옮기기 전에 wsl 을 종료 합니다.
```shell
wsl --shutdown
```

- 기존 위치에 있는 docker-desktop-data 를 옮기고 싶은 위치로 export 합니다.
- 예시는 신규 VDISK 위치를 D:/Docker 로 옮기는 경우입니다.
```shell
wsl --export docker-desktop-data "D:\Docker\docker-desktop-data.tar"
```

- 기존 위치에 있는 docker-desktop-data 를 unregister 합니다.
```shell
wsl --unregister docker-desktop-data
```

- 마지막으로 새로 옮긴 docker-desktop-data 를 import 합니다.
```shell
wsl --import docker-desktop-data "D:\Docker\" "D:\Docker\docker-desktop-data.tar" --version 2
```
