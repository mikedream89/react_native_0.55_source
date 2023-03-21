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
修改 android/settings.gradle 文件
include ':ReactAndroid'
project(':ReactAndroid').projectDir = new File(rootProject.projectDir, '../node_modules/react-native/ReactAndroid')
复制代码


修改 android/local.properties 文件（如果没有则自己创建）
sdk.dir=XXX
ndk.dir=XXX
复制代码
这里需要注意的

ndk 版本需要下载对应的，网上查到的都是比较久的版本，对应 r10b 的 NDK 。而在我最新的项目中使用了 "react-native": "0.63.3" 使用 r20b 的 NDK。
如果在项目中多个依赖库需要用到不同的 NDK 版本，我们最好还是老老实实的在 node_modules/react-native/ReactAndroid/build.gradle 中指定 NDK 版本。



修改 android/app/build.gradle 文件
android {
    // 添加上这一行
    configurations.all {
        exclude group: 'com.facebook.react', module: 'react-native'
    }
}
dependencies {
    // 把这一行替换成 
    // implementation "com.facebook.react:react-native:+" 
    implementation project(':ReactAndroid')
}

复制代码


修改 android/build.gradle 文件
dependencies {
    // 添加上这一行
    classpath 'de.undercouch:gradle-download-task:3.4.3'
}
allprojects {
    configurations.all {
        resolutionStrategy {
            dependencySubstitution {
                substitute module("com.facebook.react:react-native:+") with project(":ReactAndroid")
            }
        }
    }
}
复制代码


修改完全部文件之后，需要清理一下缓存： cd android & ./gradlew clean 
这时候重新构建，react-native 就是从 node_modules 下编译的了。可以直接修改其中的源码来达到目的。
