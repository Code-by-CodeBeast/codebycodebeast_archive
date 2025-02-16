# 3. Setting Up Environment-Based Configuration

**Status:** **IN PROGRESS**

**File:** course_series/01-flutter-foundations/02-setting-up-environment-based-config.md

**Title:**
Setting Up Environment-Based Configuration in Flutter

**Description:**
In this second installment of the "Flutter Foundations" series, we'll explore how to implement environment-based configuration for your Flutter applications. Learn how to manage different environments (development, staging, production) effectively using the ENVied package. We'll cover setting up environment variables, secure configuration management, and implementing a robust environment switching system. Perfect for developers looking to establish professional-grade configuration management in their Flutter projects.

**Tags:**
Flutter environment configuration, Flutter env setup, ENVied Flutter, environment variables Flutter, Flutter development environment, Flutter staging environment, Flutter production environment, secure configuration Flutter, Flutter config management, Flutter environment switching, Flutter app configuration, Flutter environment best practices, Flutter environment variables, Flutter development setup, Flutter configuration security, Flutter environment management, Flutter build flavors, Flutter environment separation, Flutter configuration patterns, Flutter secure env, Flutter environment architecture.

### Introduction

Managing different environments (development, staging, production) is crucial for professional Flutter applications. In this guide, we'll implement a robust environment configuration system using the ENVied package, which provides type-safe, secure access to environment variables.

For this tutorial, we'll be building upon our previous setup using Very Good CLI. We'll demonstrate how to implement environment configuration in both a package-based architecture and a standalone Flutter application.

### Setting Up the Environment Structure

#### 1. Creating Environment Files

First, create a `credentials` directory in your project's root with separate environment files:

```
|-(root of package)
|- credentials
    |- .env.development
    |- .env.production
    |- .env.staging
```

> **Important:** Add these files to your `.gitignore` to prevent sensitive information from being committed:

```gitignore
# Environment files
credentials/.env.*
```

Here's an example of how your environment files should look:

```properties:.env.development
ENVIRONMENT=development
API_URL=https://api.dev.example.com
API_KEY=dev_api_key_123
```

```properties:.env.staging
ENVIRONMENT=staging
API_URL=https://api.staging.example.com
API_KEY=staging_api_key_456
```

```properties:.env.production
ENVIRONMENT=production
API_URL=https://api.production.example.com
API_KEY=production_api_key_789
```

#### 2. Setting Up ENVied

Add the required dependencies to your `pubspec.yaml`:

```yaml
dependencies:
  envied: ^0.5.3

dev_dependencies:
  envied_generator: ^0.5.3
  build_runner: ^2.4.8
```

Run:

```bash
flutter pub get
```

#### 3. Implementing Environment Classes

Create a new file at `lib/src/env/env.dart`. This code defines three environment-specific classes that map to our .env files:

```dart:lib/src/env/env.dart
import 'package:envied/envied.dart';

part 'env.g.dart';

@Envied(path: 'credentials/.env.development', obfuscate: true)
abstract class DevEnv {
  @EnviedField(varName: 'API_URL')
  static String apiUrl = _DevEnv.apiUrl;

  @EnviedField(varName: 'API_KEY')
  static String apiKey = _DevEnv.apiKey;

  @EnviedField(varName: 'ENVIRONMENT')
  static String environment = _DevEnv.environment;
}

@Envied(path: 'credentials/.env.staging', obfuscate: true)
abstract class StgEnv {
  @EnviedField(varName: 'API_URL')
  static String apiUrl = _StgEnv.apiUrl;

  @EnviedField(varName: 'API_KEY')
  static String apiKey = _StgEnv.apiKey;

  @EnviedField(varName: 'ENVIRONMENT')
  static String environment = _StgEnv.environment;
}

@Envied(path: 'credentials/.env.production', obfuscate: true)
abstract class ProdEnv {
  @EnviedField(varName: 'API_URL')
  static String apiUrl = _ProdEnv.apiUrl;

  @EnviedField(varName: 'API_KEY')
  static String apiKey = _ProdEnv.apiKey;

  @EnviedField(varName: 'ENVIRONMENT')
  static String environment = _ProdEnv.environment;
}
```

Each class (`DevEnv`, `StgEnv`, `ProdEnv`) uses the `@Envied` decorator to:

- Specify the path to its corresponding .env file
- Enable obfuscation for security
- Create static getters for each environment variable
- Generate type-safe access to the variables through the generated part file

#### 4. Creating the Configuration Manager

Create `lib/src/env/config.dart`. This class provides a clean interface to access environment variables and manage environment switching:

```dart:lib/src/env/config.dart
enum Environment { development, staging, production }

class EnvConfig {
  static Environment _environment = Environment.development;

  static void setEnvironment(Environment env) {
    _environment = env;
  }

  static String get apiUrl {
    switch (_environment) {
      case Environment.development:
        return DevEnv.apiUrl;
      case Environment.staging:
        return StgEnv.apiUrl;
      case Environment.production:
        return ProdEnv.apiUrl;
    }
  }

  static String get apiKey {
    switch (_environment) {
      case Environment.development:
        return DevEnv.apiKey;
      case Environment.staging:
        return StgEnv.apiKey;
      case Environment.production:
        return ProdEnv.apiKey;
    }
  }

  static String get environment {
    switch (_environment) {
      case Environment.development:
        return DevEnv.environment;
      case Environment.staging:
        return StgEnv.environment;
      case Environment.production:
        return ProdEnv.environment;
    }
  }
}
```

Let's break down what this configuration manager does:

- Defines an enum `Environment` to type-safely represent our three environments
- Maintains a private static `_environment` variable to track the current environment
- Provides a `setEnvironment` method to switch environments
- Implements getter methods that automatically return the correct value based on the current environment
- Uses switch statements to ensure type-safety and handle all possible environment cases

#### 5. Exporting Configuration Classes

If you're using a package-based architecture, update your main library file (`lib/<package_name>.dart`). This file makes your environment configuration accessible to other parts of your application:

```dart:lib/flutter_starter_core.dart
/// Flutter Starter Package
library;

export 'src/env/config.dart';
export 'src/env/env.dart';
export 'src/flutter_starter_core.dart';
```

The exports ensure that:

- Other packages can import your environment configuration
- The configuration interface is clean and well-defined
- Internal implementation details remain hidden

#### 6. Generating Environment Code

Run the build_runner to generate the necessary code. This step:

- Creates the part files needed by ENVied
- Generates type-safe accessors for your environment variables
- Implements the obfuscation logic to secure your sensitive data

```bash
dart run build_runner build --delete-conflicting-outputs
```

#### 7. Updating Main Entry Points

Update your main files to use the correct environment. Each main file is responsible for:

- Setting the appropriate environment before the app starts
- Ensuring the correct configuration is used throughout the app's lifecycle
- Maintaining separation between different environment builds

```dart:lib/main_development.dart
import 'package:flutter_starter/app/app.dart';
import 'package:flutter_starter/bootstrap.dart';
import 'package:flutter_starter_core/flutter_starter_core.dart';

void main() {
  EnvConfig.setEnvironment(Environment.development);
  bootstrap(() => const App());
}
```

```dart:lib/main_staging.dart
import 'package:flutter_starter/app/app.dart';
import 'package:flutter_starter/bootstrap.dart';
import 'package:flutter_starter_core/flutter_starter_core.dart';

void main() {
  EnvConfig.setEnvironment(Environment.staging);
  bootstrap(() => const App());
}
```

```dart:lib/main_production.dart
import 'package:flutter_starter/app/app.dart';
import 'package:flutter_starter/bootstrap.dart';
import 'package:flutter_starter_core/flutter_starter_core.dart';

void main() {
  EnvConfig.setEnvironment(Environment.production);
  bootstrap(() => const App());
}
```

### Using Environment Variables

You can now access your environment variables anywhere in your code:

```dart
import 'package:flutter_starter_core/flutter_starter_core.dart';

void someFunction() {
  final apiUrl = EnvConfig.apiUrl;
  final apiKey = EnvConfig.apiKey;
  final environment = EnvConfig.environment;

  // Use these values as needed
}
```

### Troubleshooting

#### Environment Changes Not Reflecting

If modifications to your .env files aren't being picked up, this is a known issue (dart-lang/build#967). Clean the build cache and regenerate:

```bash
dart run build_runner clean
dart run build_runner build --delete-conflicting-outputs
```

### Conclusion

You now have a robust, type-safe environment configuration system for your Flutter application. This setup allows you to:

- Securely manage environment-specific variables
- Easily switch between different environments
- Keep sensitive information out of your codebase
- Maintain type safety for your configuration

In the next article, we'll explore how to implement custom app icons and splash screens for different environments.
