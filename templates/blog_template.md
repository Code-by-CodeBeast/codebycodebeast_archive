# Setting Up Your Flutter Application 
**Status:** **IN PROGRESS**
...status goes from IN PROGRESS, REVIEW, PUBLISHED

**File:** course_series/01-flutter-foundations/01-setting-up-your-flutter-app.md
.. This is the file location

**Title:** 
# Setting Up Your Flutter Application 
...This is a title

...Generate SEO Tags for content
**SEO Tags:**
Mastering Flutter, Crafting High-Performance Mobile Applications, Ultimate Guide to Flutter Mobile Development

**Description:**
In this first installment of the “Flutter Foundations: Essential Techniques and Features for High-Performing Flutter Apps” series, we’re kicking off with a hands-on guide to setting up your Flutter project using the Very Good CLI. Discover why I rely on Very Good Core for a solid foundation and learn how to configure run settings for both Android and iOS. By the end of this article, you’ll have a robust and well-structured app ready for development. Let’s get started on building something amazing!
...context not too long


...Content starts here (Use H3 to begin sections and h4 for sub-sections)

### Introduction
...This is intro


...This is a section
### Creating a Flutter application with Very Good Core
We’ll be leveraging the powerful tools and best practices developed by Very Good Ventures (VGV) to create our Flutter application. Very Good Ventures is a renowned software consultancy that specializes in Flutter development. They have contributed significantly to the Flutter ecosystem. One of their standout contributions is the Very Good CLI, which simplifies creating your Flutter application with an opinionated architecture that ensures high-quality, maintainable code.


... This is a subsection
##### Why Very Good Core?
VGV provides two frameworks: Very Good Start and Very Good Core. I wish I knew why “Very Good” needs to be in every name. For this guide, we’ll be using Very Good Core, and you guessed it—it’s because it’s the free framework! But don’t let the price tag fool you; Very Good Core is packed with powerful features that set a solid foundation for any Flutter project.  Here’s what you get with a Very Good Core:

...List has *
* Multi-Platform Support - Support for iOS, Android, Web, and Windows (macOS and Linux coming soon!)

* Build Flavors - Multiple flavor support for development, staging, and production

* Internationalization Support - Internationalization support using synthetic code generation to streamline the development process

* Sound Null Safety - No more null-dereference exceptions at runtime. Develop with a sound, static type system.

* [Bloc](https://pub.dev/packages/bloc) - Layered architecture with bloc for scalable, testable code which offers a clear separation between business logic and presentation
...This is a link


Note here that Very Good CLI requires Dart `">=3.1.0 <4.0.0"` ...Single line code 
```bash 
dart pub global activate very_good_cli
```
...Multi line code

#### 2. Create apps with Very Good CLI
##### * Creating a flutter package 
...You can use list with h5


...Blog must have a conclusion
### Conclusion
Now that you have both apps running, you’ll realize that VGV did a good job of adding splash screens and app icons but that’s not what we want. I’m going to walk you through how to have custom app icons and splash screens.


...Might need troubleshooting and error fixes (Cacn use Stack overflow for this)
### Troubleshooting and Fixing errors
####  Error 1: Occurs when you use underscore in app name in Very Good CLI's create flutter app command.
```bash
Does not match the module's namespace Error
This usually happens because your app name with an underscore is replaced with a dot as you can see in the error log below
Launching lib/main_development.dart on sdk gphone64 arm64 in debug mode...
Running Gradle task 'assembleDevelopmentDebug'...
/Users/apple/Documents/CODE_PROJECTS/FLUTTER/flutter_starter/flutter_starter/android/app/src/debug/AndroidManifest.xml Error:
	Overlay manifest:package attribute declared at AndroidManifest.xml:2:5-54 value=(com.flutter.starter.org.flutter_starter)
	does not match the module's namespace (com.flutter_starter.org.flutter_starter).
	Suggestion: remove the overlay declaration at AndroidManifest.xml.
	If you want to customize the applicationId for a buildType or productFlavor, consider setting applicationIdSuffix or using the Variant API.
	For more information, see https://developer.android.com/studio/build/build-variants

FAILURE: Build failed with an exception.

\* What went wrong:
Execution failed for task ':app:processDevelopmentDebugMainManifest'.
\> Manifest merger failed : Overlay manifest:package attribute declared at AndroidManifest.xml:2:5-54 value=(com.flutter.starter.org.flutter_starter)
  	does not match the module's namespace (com.flutter_starter.org.flutter_starter).
  	Suggestion: remove the overlay declaration at AndroidManifest.xml.
  	If you want to customize the applicationId for a buildType or productFlavor, consider setting applicationIdSuffix or using the Variant API.
  	For more information, see https://developer.android.com/studio/build/build-variants

\* Try:
\> Run with --stacktrace option to get the stack trace.
\> Run with --info or --debug option to get more log output.
\> Run with --scan to get full insights.

\* Get more help at https://help.gradle.org

BUILD FAILED in 3m 34s
Error: Gradle task assembleDevelopmentDebug failed with exit code 1
```

#### Fix:
Change the package name in these 5 locations
1. android/app/src/debug/AndroidManifest.xml
2. android/app/src/main/AndroidManifest.xml
3. android/app/src/profile/AdroidManifest.xml
4. android/app/build.gradle file defaultConfig { applicationId ""}
5. MainActivity.java on "package" OR MainActivity.kotlin android/app/src/main/kotlin/com/flutter/starter/org/MainActivity.kt


...Attach useful or references
#### Useful Links
* Very Good CLI's official documentation [here](https://cli.vgv.dev/docs/overview)
* Learn more about [Very Good Start](https://verygood.ventures/solution/very-good-start)