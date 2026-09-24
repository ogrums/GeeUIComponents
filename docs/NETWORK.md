# Cloud URL

`https://yourservice.com` is not a named constant. It is a string literal repeated in `Components/src/main/java/com/letianpai/robot/components/network/nets/GeeUINetworkUtil.java`. Both the Chinese and the overseas branch use that same literal. `isChinese` only changes the `country` header (`cn` or `global`), not the host.

## How the URL is built

Callers pass a path string called `uri`. Nothing else is inserted between the host and that string.

```text
https://yourservice.com + uri + ?sn=<serial>&ts=<timestamp>
```

Example, if `uri` is `/robot_api/v1/cloudFile/getToken`:

```text
https://yourservice.com/robot_api/v1/cloudFile/getToken?sn=...&ts=...
```

`Authorization` is a header, not part of `uri`. POST bodies (JSON map) are also not part of `uri`.

`HttpUrl.parse` returns null when `uri` contains a space or is itself a full URL. The following `.newBuilder()` then crashes. Do not put `http://10.0.2.2:8080` in `uri`. To aim at the local mock, replace the host literal in `GeeUINetworkUtil` and keep `uri` as a path that starts with `/`.

One GET adds `package_name` when `uri` equals `GeeUINetworkConsts.GET_APP_BG_INFO`.

## Path constants

The path values live in `GeeUINetworkConsts`. Every one of them is still the placeholder `your interface url`, so the real path was removed before open source. A few comments still name the old swagger operation.

| Constant | Meaning | Recovered path |
|---|---|---|
| `CALENDAR_LIST` | Calendar | unknown |
| `COUNTDOWN_LIST` | Countdown list | unknown |
| `FANS_INFO_LIST` | Fan accounts | unknown |
| `GENERAL_INFO` | Home summary | unknown |
| `CUSTOM_WATCH_CONFIG` | Watch face config | unknown |
| `CLOUD_FILE_TOKEN` | Cloud upload token | `/robot_api/v1/cloudFile/getToken` |
| `WEATHER_INFO` | Weather | unknown |
| `STOCK_INFO` | Stocks | unknown |
| `IS_DEVICE_BIND` | Bind status | unknown |
| `GET_REGION_BY_DEVICE_IP` | Region from device IP | `/robot_api/v1/device/ip/getRegion` |
| `CUSTOM_LIST` | Custom list | unknown |
| `CUSTOM_PHOTO_LIST` | Photo album | unknown |
| `COMMEMORATION_LIST` | Anniversaries | unknown |
| `LAMP_CUSTOM_INFO` | Marquee text | unknown |
| `NEWS_LIST` | News | unknown |
| `GET_SN_BY_MAC` | Serial and hardcode | `/robot_api/v1/bind/getSnByMac` (same name in LtpNetWork) |
| `GET_SESSION_TOKEN` | AWS session | `/robot_api/v1/cloudFile/getSessionToken` |
| `GET_MEDITATION_CONFIG` | Meditation config | unknown |
| `CLOCK_LIST` | Alarms | unknown |
| `GET_ALL_CONFIG` | Full robot config | unknown |
| `GET_USER_APPS_CONFIG` | User-installed apps | unknown |
| `GET_APPS_SHOW_CONFIG` | Auto-shown apps | unknown |
| `UPLOAD_STATUS` | Status upload | unknown |
| `UPLOAD_BATTERY_STATUS` | Battery status | unknown |
| `UPLOAD_LEX_LOG` | Voice log | unknown |
| `GET_COMMON_CONFIG` | Shared config | unknown |
| `POST_MODULE_CHANGE` | Display module switch | unknown |
| `POST_RESET_STATUS` | Reset robot state | unknown |
| `GET_ALL_APP_LIST` | Robot app list | unknown |
| `GET_APP_LIST` | Device app list | unknown |
| `GET_RECHARGE_CONFIG` | Auto-recharge config | unknown |
| `GET_APP_BG_INFO` | Background list | unknown |
| `GET_LATEST_PACKAGE` | Latest package | `/robot_api/v1/ota/getLatestPackage` (LtpNetWork) |
| `GET_BIND_CODE` | Bind code | `/robot_api/v1/device/code/getInfo` |
| `POST_UPLOAD_APP_STATUS` | App status | unknown |
| `POST_UPLOAD_USER_APP_STATUS` | User app status | unknown |
| `GET_USER_REMIND_LIST` | Reminders | unknown |
| `GET_SERVER_TIME_STAMP` | Server time | unknown |
| `GET_TOMATO_LIST` | Pomodoro list | unknown |
| `GET_LAST_PACKAGE` | Latest OTA package | same idea as `GET_LATEST_PACKAGE` |
| `POST_MANAGE_ADD` | Bind the robot | unknown |
| `GET_DEVICE_CHANNELLOGO` | Channel logo | unknown |

## GeeUISetting

GeeUISetting does not read `GeeUINetworkConsts`. Each screen passes the literal `"your interface url"` as `uri`, so every setting hits the same invalid URL. There is no Kotlin interface and no host field in that app.
