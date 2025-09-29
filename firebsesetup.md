## 📅 Date: 2025-09-28

### Tools Overview

| Tool                            | Purpose                                                                                                         |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Firebase Console (web)**      | Used to create and manage your Firebase project (e.g., enable Firestore, Auth, etc.)                            |
| **flutterfire configure (CLI)** | Used in your terminal to link your Flutter app to Firebase and generate all required config files automatically |

For Firebase console setup, check the [firebase_settings.md](firebase_settings.md) file.

---

## What is `flutterfire configure`?

The `flutterfire configure` command is **required** when using Firebase with a Flutter app to properly set up and integrate your project with Firebase services.

Here’s **why it’s required** and what it does:

### 🔧 What does `flutterfire configure` do?

* **Links your Flutter app with a Firebase project**:

  * It allows you to choose an existing Firebase project or create a new one.

* **Generates platform-specific Firebase configuration files**:

  * `google-services.json` for Android.
  * `GoogleService-Info.plist` for iOS/macOS.
  * `firebase_options.dart` for Dart (used by `firebase_core`).

* **Configures Firebase SDK versions**:

  * Ensures the right Firebase SDK versions and dependencies are added to your `pubspec.yaml` and native build files.

* **Supports multiple environments (optional)**:

  * Allows setting up different Firebase projects for dev, staging, and production environments using the `--project` flag.

---

### 🚫 What happens if you skip `flutterfire configure`?

* Your app won't be linked to any Firebase project.
* Platform-specific configuration files will be missing.
* Firebase initialization (`Firebase.initializeApp()`) will fail at runtime.
* You won’t be able to use services like Firestore, Auth, or Messaging.

---

### ✅ When should you run `flutterfire configure`?

* **First time setting up Firebase** in your Flutter app.
* When adding **new platforms** (e.g., adding iOS after starting with Android).
* When **switching Firebase projects** or using multiple environments.
* After **updating Firebase settings** or credentials.

---

## Common Issues & Solutions

### Issue 1: `flutterfire: command not found`

**Cause**: The FlutterFire CLI isn’t installed yet.

#### Solution:

1. **Install FlutterFire CLI via Dart pub**:

   ```bash
   dart pub global activate flutterfire_cli
   ```

2. **Add Dart's global executables to your PATH**:

   ```bash
   export PATH="$PATH":"$HOME/.pub-cache/bin"
   ```

   (Add this line to your `~/.bashrc` or `~/.zshrc` to make it permanent)

3. **Verify installation**:

   ```bash
   flutterfire --version
   ```

   If it shows a version number, you're good to go! ✅

---

### Issue 2: `Failed to fetch your Firebase projects` or `FirebaseCommandException`

**Cause**: The FlutterFire CLI requires the official Firebase CLI to be installed as well.

#### Solution:

1. **Install Firebase CLI** (for Kali/Ubuntu/Debian):

   ```bash
   curl -sL https://firebase.tools | bash
   ```

2. **Verify Firebase CLI installation**:

   ```bash
   firebase --version
   ```

   You should see a version number like `13.x.x`.

3. **Log in to Firebase**:

   ```bash
   firebase login
   ```

   A browser will open, and you’ll need to log in with the same Google account you used for your Firebase project.

4. **Retry `flutterfire configure`**:
   Run the following in your Flutter project:

   ```bash
   flutterfire configure
   ```

---

### Issue 3: Unable to see Firebase Projects

If you don't see your Firebase project, it could mean:

* You're logged into the wrong Google account.
* The project is in a different Firebase organization.

#### Solution:

1. **Log out and log in again**:

   ```bash
   firebase logout
   firebase login
   ```

2. **List Firebase projects**:

   ```bash
   firebase projects:list
   ```

   This should list your projects. If not, you'll need to create a new Firebase project.

---

## Creating a Firebase Project (if needed)

1. **Option A – From the Firebase Console** (easier to visually check):

   * Go to [Firebase Console](https://console.firebase.google.com/).
   * Click “Add project” and follow the steps (name it something unique, e.g., `salon-booking-app`).
   * Optionally, disable Google Analytics for a free project.

2. **Option B – From the Firebase CLI**:

   * Use the `firebase projects:create` command to create a new project directly from the terminal.

---

### After Configuring Firebase

Once Firebase is set up and configured, you will:

1. **Select platforms**: Android, iOS, or Web.
2. **Generate the necessary configuration files** (`lib/firebase_options.dart`).

---

### Update Your `main.dart`

After the configuration is done, update your `main.dart` to initialize Firebase with the generated options:

```dart
import 'firebase_options.dart';

await Firebase.initializeApp(
  options: DefaultFirebaseOptions.currentPlatform,
);
```

---

## Final Steps

1. After following these steps, your Flutter app should be correctly integrated with Firebase.
2. If you encounter issues, re-check your project setup, ensure you're logged into the correct Google account, and verify that the Firebase project is properly configured.

---

Good luck with your Firebase setup! 