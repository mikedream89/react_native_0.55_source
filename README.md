# react_native_0.55_source
react native 0.55.4 源码编译

settings.gradle
配置依赖源码
include ':app', ':ReactAndroid'
project(':ReactAndroid').projectDir = new File(rootProject.projectDir, '../node_modules/react-native/ReactAndroid')
app/build.gradle
//删除这行    implementation "com.facebook.react:react-native:+"  // From node_modules
implementation project(':ReactAndroid')

android/gradle.properties 配置ndk
ANDROID_NDK_VERSION=21.4.7075529
ANDROID_NDK_PATH=/Users/***/Android/sdk/ndk/21.4.7075529

打包：android目录下执行： ./gradlew clean assebleDebug
打包aar：android目录下执行：./gradlew :ReactAndroid:installArchives --no-daemon
