# 3rd-party SDK 应用指南

GAMEPOT SDK 此外，本指南还将“3rd-party SDK”应用于游戏项目而不会出现构建错误。

> 这些指南基于每个 SDK 的指南，并参考每个 SDK 指南以了解如何应用 API。

## Adjust

### Android ([Link](https://github.com/adjust/android_sdk/blob/master/doc/korean/README.md#qs-getting-started))

1. 将包添加到`build.gradle`时，已包含以下两个包。

```java
implementation 'com.android.installreferrer:installreferrer:1.0'
implementation 'com.google.android.gms:play-services-analytics:16.0.4'
```

2. 请忽略“AndroidManifest.xml”中已添加的权限。

```java
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

### iOS([Link](https://github.com/adjust/ios_sdk/blob/master/README.md))

```java
与Gamepot没有冲突。
```

### Unity

## Adbrix

### Android

### iOS

### Unity

## Singular

### Android

### iOS

### Unity

## Appsflyer

### Android

### iOS

### Unity
