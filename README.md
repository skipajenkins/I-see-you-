# ⚡ I-See-You: Real-Time Appliance Load Monitoring

A **Flutter mobile application** for monitoring appliance energy usage in real time.  
Powered by **Firebase Realtime Database** for live data, and **TensorFlow Lite** for fast, on-device AI inference.  
Designed to deliver **instant feedback**, **sliding-window AI analysis**, and **scalable monitoring** for home and industrial appliances. 📱⚡  

---

## 🌟 Features

### 📡 Real-Time Monitoring
- **Live Firebase Listeners** → Stream the latest readings from `/raw_data` without replaying history  
- **Sliding-Window Buffer** → Maintains 599 readings per device for AI input  
- **Zero-Padding Startup** → Begin inference immediately by padding until buffer is full  

### 🤖 AI-Powered Inference
- **Per-Device Models** → Each appliance uses its own `.tflite` model shipped with the app  
- **On-Device Execution** → TensorFlow Lite ensures results without cloud latency  
- **Model Readiness Check** → Optional handshake via `ai_input/models/{modelKey}/status`  
- **Latest-Only Cache** → Only the most recent inference result is displayed before clearing  

### 🖥️ Device Management
- Add/remove appliances → Synced under `/users/{uid}/devices` in Firebase  
- Toggle live analysis per device → Auto-loads model on **ON**, unloads on **OFF**  
- Seamless lifecycle → Models attach/detach listeners automatically  

### 📊 Usage & Reporting
- **Daily Totals** → Summarized usage stored under `usagePerDay`  
- **Future Analytics Ready** → Architecture supports pushing every inference to Firebase for trend reports  

---

## 🛠️ Tech Stack

| Component            | Purpose                                  |
|----------------------|------------------------------------------|
| **Flutter & Dart**   | Cross-platform mobile UI (Android & iOS) |
| **Firebase RTDB**    | Real-time data syncing                   |
| **TensorFlow Lite**  | On-device inference per appliance        |
| **Provider**         | State management in Flutter              |

---

## 🚀 Quick Start

### Prerequisites
- Flutter SDK (3.x)  
- Dart SDK (3.x)  
- Firebase project with:  
  - Realtime Database enabled  
  - Authentication enabled (Email/Password or equivalent)  
- Android Studio or Xcode for running on device/simulator  

### Installation
Clone the repository:
```bash
git clone https://github.com/I-S-U-Load-Monitoring-Application/I-see-you-.git
cd I-see-you-
flutter pub get
Configure Firebase:

Add google-services.json (Android) and/or GoogleService-Info.plist (iOS)

If needed, run FlutterFire CLI to regenerate firebase_options.dart
```

### 🔧 Configuration
Firebase Database Rules
Recommended secure rules:
```bash 
json
Copy code
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "$uid === auth.uid",
        ".write": "$uid === auth.uid"
      }
    },
    "raw_data": {
      ".read": true,
      ".write": true
    },
    "ai_input": {
      "models": {
        "$modelKey": {
          ".read": true,
          ".write": true
        }
      }
    }
  }
}
```

### Model Readiness Flags
If using readiness checks:
``` bash
json
Copy code
"ai_input": {
  "models": {
    "Fridge_tflite": { "status": "ready" },
    "TV_tflite": { "status": "ready" },
    "Microwave_tflite": { "status": "ready" },
    "Kettle_tflite": { "status": "ready" }
  }
}
```
Keys must be Firebase-safe (no ., $, #, [, ], /).

---

## 🔌 Data Flow
- Add Device → Writes to /users/{uid}/devices

- Toggle ON → Loads model, attaches listener to /raw_data

- Run Inference → Sliding window updates → TFLite processes → Result cached → UI updates

- Toggle OFF / Remove Device → Cancels listener, unloads model

---

## 📋 Requirements
### Dependencies:

- firebase_core

- firebase_auth

- firebase_database

- provider

- tflite_flutter

### Supported Platforms:
✅ Android | ✅ iOS

---

## 🛠️ Development
### Scripts
```bash
Copy code
flutter run        # Run on device/emulator
flutter build apk  # Build Android APK
flutter build ios  # Build iOS app
```

### Code Style
- Follows Dart/Flutter best practices

- Uses Provider for state management

- Separation of UI, services, and model logic

---

## 🆘 Troubleshooting
- Model not ready → Ensure status: "ready" exists in Firebase

- No data streaming → Check Firebase rules and /raw_data updates

- Output is 0 → Window still padded; wait for 599 real readings

- Performance issues → Verify .limitToLast(599) is applied and listeners detached properly

---

## 🤝 Contributing
We welcome contributions!

- Fork the repo

- Create a feature branch:

``` bash
git checkout -b feature/amazing-feature
```
- Commit your changes

- Push and open a Pull Request

---

## 📄 License
Distributed under the MIT License. See LICENSE for details.

Made with ❤️ by I-S-U-Load-Monitoring-Application
