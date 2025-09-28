Here’s your content rewritten clearly and formatted properly as a **GitHub-friendly Markdown (.md) file**, with simplified explanations and correct formatting for easy understanding:

---

````markdown
## 📅 Date: 2025-09-28

### ✅ Task
Check which devices are available for Flutter development on my system.

---

### 🧪 Step-by-Step Summary

#### 1. Check connected devices
I ran the command:
```bash
flutter devices
````

It returned:

```
Found 1 connected device:

Linux (desktop) • linux • linux-x64 • Kali GNU/Linux Rolling 6.12.33+kali-amd64
```

✅ This means only the **Linux desktop** is available as a Flutter device right now.

---

#### 2. Tried to use Firefox for web

I learned that:

> ❌ **Firefox is not recognized** by Flutter for `flutter run -d web`.
> Flutter only supports **Chrome/Chromium-based browsers** for web debugging.

---

#### 3. Locate Chromium on my system

To find where Chromium is installed, I used:

```bash
which chromium
```

It returned:

```
/usr/bin/chromium
```

---

#### 4. Tried to set Chromium manually in Flutter

I tried this command:

```bash
flutter config --chrome-executable=/usr/bin/chromium
```

But got this error:

```
Could not find an option named "--chrome-executable"
```

---

#### 5. 🛠 Solution: Create a symlink for Chromium

Flutter expects the executable to be named `google-chrome`, not `chromium`.

✅ So, I created a symbolic link:

```bash
sudo ln -s /usr/bin/chromium /usr/bin/google-chrome
```

Now Flutter can detect Chromium as if it were Chrome.

---

### 🧘 Final Notes

* Flutter only supports **Chrome/Chromium** for web development.
* Firefox won't work with `flutter run -d web`.
* On systems like **Kali Linux**, where `chromium` is installed instead of `google-chrome`, you need to create a symlink so Flutter can detect it.

---

### 🔗 Useful Commands Recap

```bash
flutter devices
which chromium
sudo ln -s /usr/bin/chromium /usr/bin/google-chrome
```