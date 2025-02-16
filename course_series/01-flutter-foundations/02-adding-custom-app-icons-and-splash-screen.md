# 2. Adding Custom App Icons and Splash Screen

**Status:** **IN PROGRESS**

**File:** course_series/01-flutter-foundations/02-adding-custom-app-icons-and-splash-screen.md

**Title:**
Adding Custom App Icons and Splash Screen in Flutter

**Description:**
In this second installment of the "Flutter Foundations" series, we'll explore how to customize your app's icons and splash screen for different build flavors (development, staging, and production). Learn two different approaches: using the flutter_launcher_icons package and manual configuration. We'll also implement a professional splash screen using flutter_native_splash. Perfect for developers looking to polish their app's first impression across different environments.

**Tags:**
Flutter app icons, Flutter splash screen, build flavors Flutter, Flutter launcher icons, Flutter app branding, Flutter icon generation, Flutter environment icons, Flutter development icons, Flutter staging icons, Flutter production icons, Flutter native splash, Flutter app initialization, Flutter app assets, Flutter icon customization, Flutter splash customization, Flutter app setup, Flutter app appearance, Flutter icon management, Flutter splash configuration, Flutter app polish.

### Introduction

A professional Flutter application needs distinct visual identities across different environments. In this guide, we'll learn how to implement custom app icons and splash screens for development, staging, and production environments. We'll explore two approaches to icon implementation and set up a native splash screen that works seamlessly across platforms.

### Adding Custom App Icons

There are two methods to implement custom app icons for different build flavors. We'll cover both approaches, but I recommend the manual method for better control over build flavors.

#### Prerequisites

Before starting either method, you'll need:

- Your app icon design in square format (e.g., 500x500 pixels)
- Different versions for each environment (dev/staging/prod)
- Icons in PNG or JPEG format

#### Method 1: Using flutter_launcher_icons Package

While this method is simpler, it has limitations with build flavors. However, it's useful for single-environment apps:

1. Add the package to your `pubspec.yaml`:

```yaml
dev_dependencies:
  flutter_launcher_icons: ^0.13.1
```

2. Create configuration in your `pubspec.yaml`:

```yaml
flutter_launcher_icons:
  android: "launcher_icon"
  ios: true
  image_path: "assets/icon/icon.png"
  min_sdk_android: 21
```

3. Run the icon generator:

```bash
flutter pub run flutter_launcher_icons
```

#### Method 2: Manual Icon Implementation (Recommended)

This method provides better control over build flavors and is recommended for professional applications.

1. Generate Icon Assets:

   - Visit [EasyAppIcon](https://easyappicon.com/)
   - Upload your icon (ensure it's square format in a 500x500 pixels)
   - Set appropriate padding and background color
   - Select "iOS + Adaptive Icons" option when downloading
   - You'll receive two sets of icons for Android and iOS

For a production-ready application, we need distinct icons for each build flavor (development, staging, production). Very Good Ventures (VGV) makes this process straightforward with their folder structure. A common practice is to add "DEV" tag to development icons, "STG" tag to staging icons, and keep production icons clean (no tags).

This visual distinction helps developers and testers quickly identify which version of the app they're using.

![See screenshot 1](https://raw.githubusercontent.com/Code-by-CodeBeast/codebycodebeast_archive/main/course_series/01-flutter-foundations/assets/02/01.png)

1. Android Icon Setup:

For Production:

```
1. Copy playstore-icon.png from downloaded icons to:
   /android/app/src/main/ic_launcher-playstore.png

2. Copy these folders from downloaded Android icons to /android/app/src/main/res:
   - mipmap-anydpi-v26
   - mipmap-hdpi
   - mipmap-mdpi
   - mipmap-xhdpi
   - mipmap-xxhdpi
   - mipmap-xxxhdpi
```

For Staging:

```
1. Copy playstore-icon.png to:
   /android/app/src/staging/ic_launcher-playstore.png

2. Copy icon folders to:
   /android/app/src/staging/res/
```

For Development:

```
1. Copy playstore-icon.png to:
   /android/app/src/development/ic_launcher-playstore.png

2. Copy icon folders to:
   /android/app/src/development/res/
```
![See screenshot 2](https://raw.githubusercontent.com/Code-by-CodeBeast/codebycodebeast_archive/main/course_series/01-flutter-foundations/assets/02/02.png)

3. iOS Icon Setup:

For Production:

```
Copy contents from downloaded iOS AppIcon.appiconset to:
/ios/Runner/Assets.xcassets/AppIcon.appiconset/
```

For Staging:

```
Copy contents to:
/ios/Runner/Assets.xcassets/AppIcon-stg.appiconset/
```

For Development:

```
Copy contents to:
/ios/Runner/Assets.xcassets/AppIcon-dev.appiconset/
```

![See screenshot 3](https://raw.githubusercontent.com/Code-by-CodeBeast/codebycodebeast_archive/main/course_series/01-flutter-foundations/assets/02/03.png)

### Implementing Splash Screen

The flutter_native_splash package provides a native splash screen experience that looks professional on both platforms.

1. Add dependency:

```yaml
dependencies:
  flutter_native_splash: ^2.4.4
```

2. Configure splash screen in `pubspec.yaml`:

```yaml
flutter_native_splash:
  color: "#000000"
  image: assets/splash-logo.png
  ios: true
  android: true
  # Additional configuration options:
  android_12:
    image: assets/splash-logo-android12.png
    icon_background_color: "#000000"
  web: false
  fullscreen: true
```

3. Generate splash screens:

```bash
dart run flutter_native_splash:create
```

4. (Optional) For different build flavors, create separate configuration files:

```yaml:flutter_native_splash_development.yaml
flutter_native_splash:
  color: "#000000"
  image: assets/splash-logo-dev.png
  android: true
  ios: true
```

```yaml:flutter_native_splash_staging.yaml
flutter_native_splash:
  color: "#000000"
  image: assets/splash-logo-stg.png
  android: true
  ios: true
```

```yaml:flutter_native_splash_production.yaml
flutter_native_splash:
  color: "#000000"
  image: assets/splash-logo.png
  android: true
  ios: true
```

Generate flavor-specific splash screens:

```bash
dart run flutter_native_splash:create --flavor development
dart run flutter_native_splash:create --flavor staging
dart run flutter_native_splash:create --flavor production
```

### Controlling App Initialization

To ensure your app's initialization completes before removing the splash screen:

```dart:lib/bootstrap.dart
import 'package:flutter_native_splash/flutter_native_splash.dart';

Future<void> bootstrap(FutureOr<Widget> Function() builder) async {
  FlutterError.onError = (details) {
    log(details.exceptionAsString(), stackTrace: details.stack);
  };

  Bloc.observer = const AppBlocObserver();

  await runZonedGuarded(
    () async {
      // Setup splash
      final widgetsBinding = WidgetsFlutterBinding.ensureInitialized();
      FlutterNativeSplash.preserve(widgetsBinding: widgetsBinding);

      // Add initialization logic here
      await Future.delayed(const Duration(seconds: 2)); // Example delay
      await loadConfigurations();
      await initializeServices();

      // Remove splash after initialization
      FlutterNativeSplash.remove();

      runApp(await builder());
    },
    (error, stackTrace) {
      log(error.toString(), stackTrace: stackTrace);
    },
  );
}
```

### Troubleshooting Common Issues

1. **Icons Not Showing After Installation**

   - Clear build cache: `flutter clean`
   - Rebuild the app
   - Verify icon paths in manifests

2. **Splash Screen Issues**

   - Ensure image paths are correct
   - Check image dimensions (recommended: 1152x1152 for Android 12)
   - Verify flavor configurations

3. **Build Flavor Problems**
   - Verify Android manifest configurations
   - Check iOS asset catalog structure
   - Ensure correct path structure in Android src folder

### Android Manifest Configuration

If you're not using the Very Good CLI setup, update your `AndroidManifest.xml`:

```xml
<application
    android:icon="@mipmap/ic_launcher"
    android:roundIcon="@mipmap/ic_launcher_round">
    <!-- ... other configurations ... -->
</application>

<adaptive-icon xmlns:android="http://schemas.android.com/apk/res/android">
    <background android:drawable="@color/ic_launcher_background" />
    <foreground android:drawable="@mipmap/ic_launcher_foreground" />
</adaptive-icon>
```

### Best Practices

1. **Icon Design**

   - Maintain consistent branding across environments
   - Use clear environment indicators for dev/staging
   - Keep production icons clean and professional

2. **Splash Screen**

   - Keep splash screen simple and branded
   - Optimize initialization time
   - Test across different device sizes

3. **Asset Management**
   - Organize assets clearly by environment
   - Version control your icon source files
   - Document any special environment configurations

### Conclusion

You now have a professional setup for managing app icons and splash screens across different environments. This implementation ensures that developers can easily identify different builds while maintaining a polished look for production releases.

In the next article, we'll explore how to set up environment-based configurations for managing different backend endpoints and app settings across environments.
