Here’s your second log entry, rewritten clearly and formatted in Markdown for GitHub. It uses **simple language**, includes **clean code blocks**, and is structured for easy understanding — perfect for your daily learning log.

---

```markdown
## 📅 Date: 2025-09-28

### 🐞 Problem

I got this error when setting up Firebase with Flutter Web:

```

FirebaseOptions cannot be null when creating the default app.

````

---

### 🧠 Why This Happens

- On **Android/iOS**, Firebase can read `google-services.json` or `GoogleService-Info.plist` automatically.
- But on **Flutter Web**, you must provide Firebase configuration **manually** using `FirebaseOptions`.

---

### ✅ Solution: Setup Firebase for Web

#### 1. Create a Firebase project

- Go to: [https://console.firebase.google.com](https://console.firebase.google.com)
- Click **“Add project”**
- Enter your **project name**
- ❌ I disabled **Gemini support** (recommended by Flutter console, but not needed now)
- ❌ I also disabled **Google Analytics** (optional, can enable later)

✅ Now the Firebase project is ready.

---

#### 2. Get Firebase Web Config

- Go to **Project Settings** → **Your apps** → **Web**
- Register your app and copy the Firebase config, which looks like this:

```js
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID",
  measurementId: "YOUR_MEASUREMENT_ID"
};
````

---

#### 3. Add FirebaseOptions in Flutter

Replace the above config with this inside your `main.dart`:

```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';

Future<void> main() async {
  WidgetsFlutterBinding.ensureInitialized();

  await Firebase.initializeApp(
    options: const FirebaseOptions(
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_PROJECT_ID.appspot.com",
      messagingSenderId: "YOUR_SENDER_ID",
      appId: "YOUR_APP_ID",
      measurementId: "YOUR_MEASUREMENT_ID",
    ),
  );

  runApp(const SalonApp());
}
```

---

### 🚀 Final Steps to Run the App

Run the following commands in your terminal:

```bash
flutter clean
flutter pub get
flutter run -d chrome
```

✅ This should launch your Flutter Web app with Firebase successfully configured.

---

### 🔗 Useful Links

* [Firebase Console](https://console.firebase.google.com)
* [FlutterFire Docs (Firebase for Flutter)](https://firebase.flutter.dev)

---

### 🧘 Reflection

* Firebase setup for web is **not automatic** — you must use `FirebaseOptions`.
* The Flutter error message was clear, and following the console's recommendations helped.

```

---

This version is ready to be copied into your `2025-09-28.md` file in your GitHub log repo.

Would you like me to bundle both logs into a single `.md` file for you?
```
