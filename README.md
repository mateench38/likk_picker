<h1 align="center">Likk Picker</h1>

<p align="center">
  <a href="https://flutter.dev">
    <img src="https://img.shields.io/badge/Platform-Flutter-02569B?logo=flutter" alt="Platform" />
  </a>
  <a href="https://pub.dartlang.org/packages/likk_picker">
    <img src="https://img.shields.io/pub/v/likk_picker.svg" alt="Pub Package" />
  </a>
  <a href="https://pub.dev/packages/likk_picker/score">
    <img src="https://badges.bar/likk_picker/likes" alt="likes"/>
  </a>
  <a><img src="https://img.shields.io/github/forks/tranhuongk/likk_picker" alt="Forks"/></a>
</p>


<center><a href="https://www.paypal.com/paypalme/tranhuongk" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy Me A Coffee" style="height: 41px !important;width: 174px !important;box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;" ></a></center>

---


<p align="center">A flutter package which is clone of facebook messenger gallery picker and camera, combined as single component. Gallery view and Camera view can also be use as Flutter widget. Under the hood likk picker used <a href="https://pub.dev/packages/photo_manager">Photo Manager</a> and <a href="https://pub.dev/packages/camera">Camera</a>. Custom from <a href="https://pub.dev/packages/drishya_picker">Drishya Picker</a>.</p>

---

# Table of contents

- [Installing](#installing)
- [Platform Setup](#platform-setup)
  - [Android](#android)
  - [IOS](#ios)
- [Gallery](#gallery)
- [Camera](#camera)
- [Bugs or Requests](#bugs-or-requests)
- [Contributors](#contributors)

---

# Installing

### 1. Add dependency
Add this to your package's `pubspec.yaml` file:
```yaml
dependencies:
  likk_picker: ^latest_version
```

### 2. Install it
You can install packages from the command line:

with `pub`:
```
$ pub get
```
with `Flutter`:
```
$ flutter pub get
```

### 3. Import it
Now in your `Dart` code, you can use:
```dart
import 'package:likk_picker/likk_picker.dart';
```

---

# Platform Setup

For more details (if needed) you can go through <a href="https://pub.dev/packages/photo_manager">Photo Manager</a> and <a href="https://pub.dev/packages/camera">Camera</a> readme section as well.

## Android

Change the minimum Android sdk version to 21 (or higher) in your `android/app/build.gradle` file.

```
minSdkVersion 21
```

Required permissions: `INTERNET`, `READ_EXTERNAL_STORAGE`, `WRITE_EXTERNAL_STORAGE`, `ACCESS_MEDIA_LOCATION`.
If you don't need the `ACCESS_MEDIA_LOCATION` permission,
see [Disable `ACCESS_MEDIA_LOCATION` permission](#disable-access_media_location-permission).

### glide

Android native use glide to create image thumb bytes, version is 4.11.0.

If your other android library use the library, and version is not same, then you need edit your android project's build.gradle.

```gradle
rootProject.allprojects {

    subprojects {
        project.configurations.all {
            resolutionStrategy.eachDependency { details ->
                if (details.requested.group == 'com.github.bumptech.glide'
                        && details.requested.name.contains('glide')) {
                    details.useVersion '4.11.0'
                }
            }
        }
    }
}
```

And, if you want to use ProGuard, you can see the [ProGuard of Glide](https://github.com/bumptech/glide#proguard).

### Remove Media Location permission

Android contains [ACCESS_MEDIA_LOCATION](https://developer.android.com/training/data-storage/shared/media#media-location-permission) permission by default.

This permission is introduced in Android Q. If your app doesn't need this permission, you need to add the following node to the Android manifest in your app.

```xml
<uses-permission
  android:name="android.permission.ACCESS_MEDIA_LOCATION"
  tools:node="remove"
  />
```

If you found some warning logs with `Glide` appearing,
then the main project needs an implementation of `AppGlideModule`. 
See [Generated API](https://sjudd.github.io/glide/doc/generatedapi.html).

## IOS

Add following content to `info.plist`.

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Replace with your permission description..</string>
<key>NSCameraUsageDescription</key>
<string>Replace with your permission description..</string>
<key>NSMicrophoneUsageDescription</key>
<string>Replace with your permission description..</string>
```

---

# Gallery

<div style="text-align: center">
    <table>
        <tr>
            <td style="text-align: center">
                <img src="https://raw.githubusercontent.com/tranhuongk/likk_picker/master/assets/gallery.gif" width="200"/>
            </td>            
            <td style="text-align: center">
                <img src="https://raw.githubusercontent.com/tranhuongk/likk_picker/master/assets/1.jpg" width="200"/>
            </td>
            <td style="text-align: center">
                <img src="https://raw.githubusercontent.com/tranhuongk/likk_picker/master/assets/7.jpg" width="200" />
            </td>
        </tr> 
    </table>
</div>
<div style="text-align: center">
    <table>
        <tr>
            <td style="text-align: center">
                <img src="https://raw.githubusercontent.com/tranhuongk/likk_picker/master/assets/photo_editor.gif" width="200"/>
            </td>            
            <td style="text-align: center">
                <img src="https://raw.githubusercontent.com/tranhuongk/likk_picker/master/assets/5.jpg" width="200"/>
            </td>
            <td style="text-align: center">
                <img src="https://raw.githubusercontent.com/tranhuongk/likk_picker/master/assets/6.jpg" width="200" />
            </td>
        </tr> 
    </table>
</div>


 1. Use `GalleryViewWrapper` to make gallery view collapsible otherwise ignore it.

 ```dart
class PickerDemo extends StatelessWidget {
  late final GalleryController controller;
  
  ...

  @override
  Widget build(BuildContext context) {
    return GalleryViewWrapper(
      controller: controller,
      child: Scaffold(
        body: ...
      ),
    );
  }
}
``` 
 2. `GalleryController` can be used for extra setting and 
    picking media.

- Using `pick()` function on controller to pick media.

```dart
class PickerDemo extends StatelessWidget {
  late final GalleryController controller;

  @override
  void initState() {
    super.initState();
    controller = GalleryController(
      gallerySetting: const GallerySetting(
        albumSubtitle: 'Collapsable',
        enableCamera: true,
        maximum: 10,
        requestType: RequestType.all,
      ),
      panelSetting: const PanelSetting(topMargin: 24.0),
    );
  }

  ...

  onPressed : () async {
    final entities = await controller.pick();
  }

  ...
}
```

 3. Using `GalleryViewField` similarly as flutter `TextField` to pick media.

- `onChanged` – triggered every time user select/unselect media

- `onSubmitted` – triggered when user done with selection


```dart
GalleryViewField(
  selectedEntities: [],
  onChanged: (entity, isRemoved) {
     ...
  },
  onSubmitted: (list) {
     ...
  }
  child: const Icon(Icons.camera),
),
```

4. You can also use `GalleryView` as a `Widget`.

---

# Camera

<div style="text-align: center">
    <table>
        <tr>
            <td style="text-align: center">
                <img src="https://raw.githubusercontent.com/tranhuongk/likk_picker/master/assets/camera.gif" width="200"/>
            </td>            
            <td style="text-align: center">
                <img src="https://raw.githubusercontent.com/tranhuongk/likk_picker/master/assets/8.jpg" width="200"/>
            </td>
            <td style="text-align: center">
                <img src="https://raw.githubusercontent.com/tranhuongk/likk_picker/master/assets/12.jpg" width="200" />
            </td>
        </tr> 
    </table>
</div>
<div style="text-align: center">
    <table>
        <tr>
            <td style="text-align: center">
                <img src="https://raw.githubusercontent.com/tranhuongk/likk_picker/master/assets/playground.gif" width="200"/>
            </td>            
            <td style="text-align: center">
                <img src="https://raw.githubusercontent.com/tranhuongk/likk_picker/master/assets/photo_editor.gif" width="200"/>
            </td>
            <td style="text-align: center">
                <img src="https://raw.githubusercontent.com/tranhuongk/likk_picker/master/assets/10.jpg" width="200" />
            </td>
        </tr> 
    </table>
</div>

 1. Using `pick()` function on `CameraView` to pick media.

```dart
  ...
  onPressed : () async {
    final entity = await CameraView.pick();
  }
  ...
```

 2. Using `CameraViewField` similarly as flutter `TextField` to pick media.

- `onCapture` – triggered when photo/video capture completed

```dart
GalleryViewField(
  onCapture: (entity) {
     ...
  },
  child: const Icon(Icons.camera),
),
```

3. You can also use `CameraView` as a `Widget`.

---
# Bugs or Requests

If you encounter any problems feel free to open an [issue](https://github.com/tranhuongk/likk_picker/issues/new?template=bug_report.md). If you feel the library is missing a feature, please raise a [ticket](https://github.com/tranhuongk/likk_picker/issues/new?template=feature_request.md) on GitHub and I'll look into it. Pull request are also welcome.

---

# Maintainers

- [Huong Tran](https://tranhuongk.xyz/)
