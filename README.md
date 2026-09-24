# GeeUIComponets

Client libraries used by the system apps. The repository name is spelled `GeeUIComponets`. `settings.gradle` sets the Gradle project name to `GeeUIComponents`.

## Modules

| Module | Role |
|---|---|
| `CommChannel` | AIDL contract `ILetianpaiService` and the parcel `LtpCommand` (`command` + `data`). This repo does not bind or implement the service |
| `Components` | OkHttp helpers, expression path lookup, Wi-Fi connect helper, preferences, logs under `sdcard/letianpai/.log` |
| `GeeUIWidget` | Keypad-style `Button` / `ImageButton` views (`BackButton`, `HomeButton`, `SettingsButton`, `KeyButton`) |
| `app` | Sample activity only |

`settings.gradle` points the modules at `GeeUIComponents/...`. On disk the folders are `CommChannel/`, `Components/`, and `GeeUIWidget/`. A fresh checkout will not compile until those `projectDir` paths match the folders.

## Binder API

`ILetianpaiService` is the bus every system app uses:

- `getRobotStatus` / `setRobotStatus`
- `setCommand(LtpCommand)` plus `registerCallback` / `unregisterCallback`
- Paired set and register methods for long-connect, MCU, audio effect, expression, app command, robot status, TTS, speech, sensor, Xiaomi, identify, and BLE

`RobotAidlConsts` numbers the channels (`CMD_LONG_CONNECT` through `CMD_BLE_RESPONSE`, plus resume and pause). `LetianpaiService` is the process that implements this stub.

## HTTP helpers

`GeeUiNetManager` wraps OkHttp calls: device info, calendar, weather, fans, news, OTA package, bind status, upload token, expression list, robot status. Callers pass a `Context`, often `boolean isChinese`, and an OkHttp `Callback`.

`GeeUINetworkConsts` path strings are the placeholder `"your interface url"`. They are not live hosts. Typed Retrofit calls live in `LtpNetWork`.

`ExpressionCenter.getExpressionPath(name)` queries `content://com.letianpai.robot.resources.provider/expression` for the `.mp4` of that face. The provider is another app (`geeuiresources`), not this library.

## Comment glossary

| Where | Chinese | English |
|---|---|---|
| `GeeUINetworkConsts` | 日历 / 倒计时列表 / 获取粉丝信息 / 天气信息 | Calendar / countdown list / fan info / weather |
| `GeeUINetworkConsts` | 获取云文件上传凭证 / 设备绑定状态 / 获取机器人全部配置 | Cloud upload token / bind status / full robot config |
| `ExpressionCenter` | 切换下一个表情 | Switch to the next expression |
| `PackageConsts` | 拍照需要启动的服务 | Service that must be started to take a photo |
| `GeeUIStatusUploader` | 获取当前wifi名字 / 获取蓝牙地址 / 更新机器人状态 | Current Wi-Fi name / Bluetooth address / update robot status |
| `WIFIConnectionManager` | 尝试连接指定wifi | Try to connect to the given Wi-Fi |
| `VolumeManager` | 睡眠模式管理器 | Comment says "sleep-mode manager"; the class is volume-related |
| `GeeUILogUtils` | 指定日志文件的目录路径 / 日志文件存活时间，单位毫秒 | Log directory / how long log files are kept, in milliseconds |

## Build

AGP 7.3.0. `Components` minSdk 26, OkHttp 4.10.0, Gson, Picasso, ZXing, xlog.
