# DS Audio

> A music application for managing music on Synology NAS.

## Requirements

### iOS

1. 导入证书
```shell
security import ./<name>.cer -P <password> -A -t cert
```

2. 导入.p12私钥
```shell
security import ./<name>.p12 -P <password> -A -t cert -f pkcs12
```

3. 双击导入mobileprovision文件
4. 手动安装模拟器运行时
- 已安装列表
```shell
xcrun simctl runtime list
```

- 安装
```shell
xcrun simctl runtime add "~/iOS_17.0.1_Simulator_Runtime.dmg"
```

## Release

以下脚本都在`Makefile`文件下，JetBrains / Android Studio可能需要安装`Makefile Language`插件。

### Flutter

>在`pubspec.yaml`文件修改`version: <new version>`

### Android

>如果发布到生产环境, 请在`android/app/build.gradle`文件修改`versionCode`

- 如果发布到测试环境, 运行
```shell
make ENV=<dev|stg|prod> apk
```

- 如果发布到生产环境, 运行
```shell
make ENV=<dev|stg|prod> appbundle
```

### iOS

>如果发布到生产环境, 请修改`build`号

- 如果发布到测试环境, 运行
```shell
make ENV=<dev|stg|prod> adhoc
```

- 如果发布到生产环境, 运行
```shell
make ENV=<dev|stg|prod> appstore
```

### macOS

>如果发布到生产环境, 请修改`build`号

- 如果发布到测试环境, 运行
```shell
make ENV=<dev|stg|prod> macos_adhoc
```

- 如果发布到生产环境, 运行
```shell
make ENV=<dev|stg|prod> macos_appstore
```

### Linux

```shell
# https://distributor.leanflutter.dev/makers/appimage/
dart run flutter_distributor:main package --platform linux --targets appimage

# https://distributor.leanflutter.dev/makers/deb/
dart run flutter_distributor:main package --platform linux --targets deb

# https://distributor.leanflutter.dev/makers/rpm/
dart run flutter_distributor:main package --platform linux --targets rpm

# https://jrsoftware.org/isinfo.php
# https://distributor.leanflutter.dev/zh-hans/makers/exe/
flutter_distributor package --platform windows --targets exe

# https://flutter.cn/docs/platform-integration/windows/building#msix-packaging
# https://distributor.leanflutter.dev/zh-hans/makers/msix/
flutter_distributor package --platform windows --targets msix

# https://flutter.cn/docs/deployment/linux
# https://snapcraft.io/docs/snapcraft-yaml-reference
snapcraft

# Test snap
sudo snap install ./dsaudio.snap --dangerous

# build & sign pkg
# developer id installer
xcrun pkgbuild --component "DS Audio.app" --install-location /Applications --sign "" DS\ Audio.pkg

# Notarize pkg
xcrun notarytool submit --verbose DS\ Audio.pkg --apple-id "" --team-id "" --password ""
xcrun notarytool info <id> --apple-id "" --team-id "" --password ""
xcrun notarytool log <id> --apple-id "" --team-id "" --password ""
```

## Tests

### app links

#### Deep Links on Android

```shell
adb shell am start -a android.intent.action.VIEW -d "dsa://debug"
```

#### Custom URL schemes on iOS

```shell
/usr/bin/xcrun simctl openurl booted "dsa://debug"
```
