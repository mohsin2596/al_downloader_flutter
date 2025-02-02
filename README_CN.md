# al_downloader

[![pub package](https://img.shields.io/pub/v/al_downloader.svg)](https://pub.dartlang.org/packages/al_downloader)

一个基于url的Flutter下载器，支持下载任意类型的文件，并自动管理下载相关的各种事务。

## 特性

* 通过url管理下载任务
* 简单的下载状态
* 不频繁地I/O
* 提供便利的下载句柄
* 支持批量下载
* 自动管理文件，不需要指定下载路径
* 基于[flutter_downloader](https://pub.dev/packages/flutter_downloader)

## 集成

> 原生配置：和[flutter_downloader](https://pub.dev/packages/flutter_downloader)相同

添加下面这行代码到pubspec.yaml中

```
dependencies:
  al_downloader: ^1.6.6
```

使用命令行运行下面这行代码
```
flutter packages get
```

引入下面这行代码来使用al_downloader
```
import 'package:al_downloader/al_downloader.dart';
```

## 使用

### ALDownloader

```
/// 下载
await ALDownloader.download(url,
    downloaderHandlerInterface:
        ALDownloaderHandlerInterface(progressHandler: (progress) {
      debugPrint(
          "ALDownloader | 下载进度 = $progress, url = $url\n");
    }, succeededHandler: () {
      debugPrint("ALDownloader | 下载成功, url = $url\n");
    }, failedHandler: () {
      debugPrint("ALDownloader | 下载失败, url = $url\n");
    }, pausedHandler: () {
      debugPrint("ALDownloader | 下载暂停, url = $url\n");
    }));
```

```
/// 添加一个下载句柄池
ALDownloader.addDownloaderHandlerInterface(
    ALDownloaderHandlerInterface(progressHandler: (progress) {
      debugPrint(
          "ALDownloader | 下载进度 = $progress, url = $url\n");
    }, succeededHandler: () {
      debugPrint("ALDownloader | 下载成功, url = $url\n");
    }, failedHandler: () {
      debugPrint("ALDownloader | 下载失败, url = $url\n");
    }, pausedHandler: () {
      debugPrint("ALDownloader | 下载暂停, url = $url\n");
    }),
    url);
```

```
/// 添加一个持久下载句柄池
ALDownloader.addForeverDownloaderHandlerInterface(
    ALDownloaderHandlerInterface(progressHandler: (progress) {
      debugPrint(
          "ALDownloader | 下载进度 = $progress, url = $url\n");
    }, succeededHandler: () {
      debugPrint("ALDownloader | 下载成功, url = $url\n");
    }, failedHandler: () {
      debugPrint("ALDownloader | 下载失败, url = $url\n");
    }, pausedHandler: () {
      debugPrint("ALDownloader | 下载暂停, url = $url\n");
    }),
    url);
```

```
/// 移除下载句柄池
ALDownloader.removeDownloaderHandlerInterfaceForUrl(url);
```

```
/// 获取下载状态
final status = ALDownloader.getStatusForUrl(url);
```

```
/// 获取下载进度
final progress = ALDownloader.getProgressForUrl(url);
```

```
/// 暂停下载
///
/// 停止下载，不删除未下载完成的数据
await ALDownloader.pause(url);
```

```
/// 取消下载
///
/// 停止下载，删除未下载完成的数据
await ALDownloader.cancel(url);
```

```
/// 移除下载
///
/// 删除下载，删除所有数据
await ALDownloader.remove(url);
```

### ALDownloaderBatcher

```
/// 批量下载
await ALDownloaderBatcher.downloadUrls(urls,
    downloaderHandlerInterface:
        ALDownloaderHandlerInterface(progressHandler: (progress) {
      debugPrint("ALDownloader | 批量 | 下载进度 = $progress\n");
    }, succeededHandler: () {
      debugPrint("ALDownloader | 批量 | 下载成功\n");
    }, failedHandler: () {
      debugPrint("ALDownloader | 批量 | 下载失败\n");
    }, pausedHandler: () {
      debugPrint("ALDownloader | 批量 | 下载暂停\n");
    }));
```

```
/// 对批量下载添加一个下载句柄池
ALDownloaderBatcher.addDownloaderHandlerInterface(
    ALDownloaderHandlerInterface(progressHandler: (progress) {
      debugPrint("ALDownloader | 批量 | 下载进度 = $progress\n");
    }, succeededHandler: () {
      debugPrint("ALDownloader | 批量 | 下载成功\n");
    }, failedHandler: () {
      debugPrint("ALDownloader | 批量 | 下载失败\n");
    }, pausedHandler: () {
      debugPrint("ALDownloader | 批量 | 下载暂停\n");
    }),
    urls);
```

```
/// 获取一组url的下载状态
final status = ALDownloaderBatcher.getStatusForUrls(urls);
```

### ALDownloaderPersistentFileManager - 基于url的持久化文件管理器

```
final model = await ALDownloaderPersistentFileManager
    .lazyGetALDownloaderPathModelForUrl(url);
debugPrint(
    "ALDownloader | 获取[url]的“物理目录路径”和“虚拟/物理文件名”, url = $url, model = $model\n");

final path2 = await ALDownloaderPersistentFileManager
    .lazyGetAbsolutePathOfDirectoryForUrl(url);
debugPrint(
    "ALDownloader | 获取[url]的“物理目录路径”, url = $url, path = $path2\n");

final path3 = await ALDownloaderPersistentFileManager
    .getAbsoluteVirtualPathOfFileForUrl(url);
debugPrint(
    "ALDownloader | 获取[url]的“虚拟文件路径”, url = $url, path = $path3\n");

final path4 = await ALDownloaderPersistentFileManager
    .getAbsolutePhysicalPathOfFileForUrl(url);
debugPrint(
    "ALDownloader | 获取[url]的“物理文件路径”, url = $url, path = $path4\n");

final isExist = await ALDownloaderPersistentFileManager
    .isExistAbsolutePhysicalPathOfFileForUrl(url);
debugPrint(
    "ALDownloader | 检查[url]是否存在“物理文件路径”, url = $url, is Exist = $isExist\n");

final fileName = ALDownloaderPersistentFileManager.getFileNameForUrl(url);
debugPrint(
    "ALDownloader | 获取[url]的“虚拟/物理文件名”, url = $url, file name = $fileName\n");
```

### ALDownloaderPrintConfig

```
/// 开启打印
ALDownloaderPrintConfig.enabled = true;

/// 关闭频繁打印
ALDownloaderPrintConfig.frequentEnabled = false;
```

## *提示*:

*1. 在一个协程中，方法需要`await`修饰*

*例如：*
```
Future<void> executeSomeMethodsTogetherSerially() async {
  await ALDownloader.initialize();
  await ALDownloader.remove(url);
  await ALDownloader.download(url);
}
```

*2. 如果持久化文件被一些异常方式删除了，比如某些业务代码删除了缓存文件夹，调用[remove]，然后调用[download]重新下载来解决这个问题*

## Example的主要文件

### iOS

- [main.dart](https://github.com/jackleemeta/al_downloader_flutter/blob/master/example/lib/main.dart)
- [AppDelegate.swift](https://github.com/jackleemeta/al_downloader_flutter/blob/master/example/ios/Runner/AppDelegate.swift)
- [Info.plist](https://github.com/jackleemeta/al_downloader_flutter/blob/master/example/ios/Runner/Info.plist)

### Android

- [main.dart](https://github.com/jackleemeta/al_downloader_flutter/blob/master/example/lib/main.dart)
- [AndroidManifest.xml](https://github.com/jackleemeta/al_downloader_flutter/blob/master/example/android/app/src/main/AndroidManifest.xml)

> Maintainer: jackleemeta (jackleemeta@outlook.com)