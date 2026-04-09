# CallDialer & Weather Teller

A voice-driven Android assistant that lets users make phone calls, launch apps, and query weather information — all hands-free using natural speech commands.

## Overview

**CallDialer & Weather Teller** is a native Android application built in Java that brings a Siri-like voice assistant experience to Android devices. Users interact with the app via a push-to-talk microphone button; the app listens for natural language commands, resolves contacts from the device's address book, and initiates calls without requiring any manual input. Additional voice commands support launching installed applications, closing the app, and (in a planned extension) fetching live weather data. The project demonstrates the use of Android's built-in `SpeechRecognizer` API for offline-capable, real-time voice processing without relying on a third-party NLP service.

## Key Features

- 🎙️ **Push-to-talk voice input** — hold the microphone button to start listening; release to process
- 📞 **Voice-activated calling** — say *"call [name]"* to look up a contact and dial immediately
- 📱 **App launcher via voice** — say *"open the app [package name]"* to launch any installed application
- 🚪 **Voice app close** — say *"close the app"* to exit the assistant hands-free
- 🔍 **Contact resolution** — fuzzy name search against the device's native contacts using `ContactsContract`
- ☁️ **Weather querying** *(planned)* — voice trigger on the keyword *"weather"* is wired and ready for API integration
- 🔒 **Runtime permission handling** — requests `RECORD_AUDIO`, `CALL_PHONE`, and `READ_CONTACTS` at runtime per Android guidelines

## Tech Stack

| Category | Technology |
|---|---|
| **Language** | Java |
| **Platform** | Android (minSdk 30 / targetSdk 33) |
| **Build Tool** | Gradle 8 (Android Gradle Plugin 8.3.2) |
| **UI** | XML layouts · ConstraintLayout |
| **Speech** | Android `SpeechRecognizer` / `RecognizerIntent` |
| **Contacts** | Android `ContactsContract` ContentProvider |
| **Libraries** | AndroidX AppCompat 1.4.1 · Material Components 1.5.0 · ConstraintLayout 2.1.3 |
| **Testing** | JUnit 4 · Espresso · AndroidX Test |

## Installation

### Prerequisites

- Android Studio **Hedgehog (2023.1)** or later
- JDK 8+
- An Android device or emulator running **Android 11 (API 30)** or higher

### Steps

```bash
# 1. Clone the repository
git clone https://github.com/vanix056/CallDialer-and-Weather-teller.git
cd CallDialer-and-Weather-teller

# 2. Open in Android Studio
#    File → Open → select the cloned directory

# 3. Sync Gradle dependencies
#    Android Studio will prompt automatically, or run:
./gradlew build

# 4. Run on a connected device or emulator
./gradlew installDebug
```

> **Note:** A physical device is recommended for testing call and microphone features. Emulators may not support `CALL_PHONE` or live microphone input reliably.

## Usage

1. Launch the **Siri** app on your Android device.
2. Grant the requested permissions: *Microphone*, *Phone*, and *Contacts*.
3. **Hold** the microphone button to start listening.
4. Speak one of the supported commands (see below).
5. **Release** the button — the app processes your speech and acts immediately.

### Supported Voice Commands

| Command | Example | Action |
|---|---|---|
| `call [name]` | *"call Abdullah"* | Looks up the contact and dials their number |
| `weather` | *"weather"* | Triggers the weather lookup hook (integration pending) |
| `open the app [package]` | *"open the app com.spotify.music"* | Launches the specified app by package name |
| `close the app` | *"close the app"* | Closes the assistant |

## Project Structure

```
CallDialer-and-Weather-teller/
├── app/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/example/siri/nust/
│   │   │   │   ├── MainActivity.java       # Core activity: speech recognition, command routing
│   │   │   │   └── SiriListener.java       # Background service stub (planned extension)
│   │   │   ├── res/
│   │   │   │   ├── layout/
│   │   │   │   │   └── activity_main.xml   # Push-to-talk UI with status TextView
│   │   │   │   └── values/
│   │   │   │       ├── strings.xml
│   │   │   │       ├── colors.xml
│   │   │   │       └── themes.xml
│   │   │   └── AndroidManifest.xml         # Permissions & component declarations
│   │   ├── test/                           # Unit tests (JUnit 4)
│   │   └── androidTest/                    # Instrumented tests (Espresso)
│   └── build.gradle                        # Module-level dependencies
├── build.gradle                            # Project-level plugin configuration
├── settings.gradle                         # Module inclusion & repository config
└── gradle.properties
```

## Configuration

No external API keys or environment variables are required to build and run the core application.

For the **weather feature**, integrate a weather API (e.g., [OpenWeatherMap](https://openweathermap.org/api)) inside the `checkWeather()` stub in `MainActivity.java`. You will need to:
1. Add your API key (store it in `local.properties` or `BuildConfig`, never hard-coded).
2. Add a networking library (e.g., Retrofit + OkHttp) to `app/build.gradle`.
3. Implement the HTTP call and TTS/display response inside `checkWeather()`.

### Required Android Permissions

| Permission | Purpose |
|---|---|
| `RECORD_AUDIO` | Microphone access for speech recognition |
| `INTERNET` | Future weather API calls |
| `READ_CONTACTS` | Resolve contact name to phone number |
| `WRITE_CONTACTS` | Reserved for future contact management |
| `CALL_PHONE` | Initiate phone calls programmatically |

## UI Features

- **Minimalist single-screen layout** — a centered microphone `ImageButton` and a real-time `TextView` status indicator
- **Status feedback** — the status label updates at every recognition lifecycle stage: idle (`...^_^...`), listening, processing, and result display
- **No-touch flow** — after initial permission grant, the entire interaction is voice-driven

## Contributing

Contributions are welcome. Please follow these guidelines:

1. Fork the repository and create a feature branch: `git checkout -b feature/your-feature`
2. Write clean, well-commented Java code consistent with the existing style
3. Test on a physical device (API 30+) before submitting
4. Submit a pull request with a clear description of the change and its motivation

Bug reports and feature requests can be filed via [GitHub Issues](https://github.com/vanix056/CallDialer-and-Weather-teller/issues).

## License

This project is licensed under the [MIT License](LICENSE).

## Author

Developed by **[vanix056](https://github.com/vanix056)** as part of a university project at NUST, combining Android development with speech-driven UX design.

