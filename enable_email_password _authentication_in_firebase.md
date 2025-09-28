
## 📅 Date: 2025-09-28

### 🔐 Enable Email/Password Authentication in Firebase

If you don’t enable this, your user sign-up or login will always fail silently or with an auth error.

#### ✅ Steps 1:

1. Go to **[Firebase Console](https://console.firebase.google.com)**
2. Navigate to:
   `Authentication → Sign-in method`
3. Find **Email/Password** in the list.
4. Click it → **Enable**
5. Click **Save**

✅ Now your Firebase project can accept email and password logins.

---

### 🚀 Step 2: Run Your Web App

After enabling auth, run your app again:

```bash
flutter clean
flutter pub get
flutter run -d chrome
```

This ensures all dependencies are fresh and your Flutter Web app connects to Firebase properly.

---

