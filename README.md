# WhatsApp Clone — Flutter + Firebase

A **WhatsApp-style messaging app** built with **Flutter**, using **Firebase** for
authentication, real-time messaging and media storage, and **Riverpod** for state management.

---

## Highlights

- **Full Firebase stack** — Firebase Authentication for sign-in, Cloud Firestore for real-time
  conversations, and Firebase Storage for shared media.
- **Riverpod state management** — controllers expose typed, immutable state; the root widget
  watches a single `userDataAuthProvider` to decide between the landing and chat screens.
- **Rich messaging** — text and image messages via `image_picker`, **voice notes** using
  `flutter_sound`, and video playback with `cached_video_player`.
- **Device integration** — contact picker via `flutter_contacts`, country selection via
  `country_picker`, and runtime permissions handled by `permission_handler`.
- **Polish** — dark theme, shimmer loading states, an emoji picker, and cached remote images.

---

## Architecture

```
lib/
├── features/
│   ├── auth/
│   │   └── controller/auth_controller.dart   # userDataAuthProvider
│   └── landing/
│       └── screens/landing_screen.dart
├── screens/
│   └── mobile_layout_screen.dart             # Post-login chat shell
├── utils/
│   └── colors.dart
├── widgets/
│   └── error.dart
└── firebase_options.dart
```

The root widget reacts to auth state, so there is no manual navigation juggling between the
signed-out and signed-in experiences:

```dart
class MyApp extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final currentUser = ref.watch(userDataAuthProvider);
    return MaterialApp(
      title: 'Whatsapp UI',
      theme: ThemeData.dark().copyWith(scaffoldBackgroundColor: backgroundColor),
      home: currentUser.when(
        data: (user) => user == null ? const LandingScreen() : const MobileLayoutScreen(),
        error: (error, trace) => /* error widget */,
      ),
    );
  }
}
```

---

## Tech Stack

| Concern | Choice |
|---|---|
| UI | Flutter, Material, dark theme |
| Backend | Firebase Auth, Cloud Firestore, Firebase Storage |
| State management | `flutter_riverpod` |
| Media | `flutter_sound`, `audioplayers`, `cached_video_player`, `image_picker` |
| UX | `enough_giphy_flutter`, `emoji_picker_flutter`, `shimmer`, `cached_network_image` |
| Utilities | `flutter_contacts`, `country_picker`, `permission_handler`, `uuid`, `intl` |

---

## Getting Started

### Prerequisites

- Flutter `>= 3.16`
- A Firebase project with **Authentication**, **Cloud Firestore** and **Storage** enabled

### Setup

1. Create a Firebase project and register an Android app.
2. Place `google-services.json` in `android/app/`.
3. Replace the placeholder values in `lib/firebase_options.dart` with your project's config.
4. Install and run:

```bash
git clone https://github.com/developer-rahim/Chat-App.git
cd Chat-App
flutter pub get
flutter run
```

### Firestore rules

For local development, a permissive ruleset is fine. For production, scope reads to
authenticated participants of each conversation.

---

## Notes

- `firebase_options.dart` contains project identifiers only — these are safe to commit, unlike API
  keys. Never commit service-account JSON or generated `google-services.json` with private keys.
- Voice recording and the contact picker require runtime permissions on Android.

---

## License

Released under the MIT License.
