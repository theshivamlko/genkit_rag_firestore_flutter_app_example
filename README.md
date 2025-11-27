# HR Agent - GenKit Firestore Flutter Example

This repository contains an example Flutter app and a GenKit-based Firebase Functions service that demonstrates image generation/modification using GenKit and Google GenAI.

 [Youtube video](https://www.youtube.com/watch?v=Aa7VueOW_YE) 


![](sample_outpu.png)

## Contents

- `genkit_rag_service/` — GenKit-driven Firebase Functions (TypeScript). The functions use a GenKit flow to generate modified images from an input image.
- `hr_agent_flutter_app/` — Flutter example app that calls the cloud function.

## Prerequisites

- Git (to clone the repo)
- Node.js 22.x (the functions `package.json` specifies Node 22)
- npm
- Firebase CLI (for emulators and deployment)
- Flutter SDK 

Install Firebase CLI (if not installed):

```powershell
npm install -g firebase-tools
firebase login
```

Install Node 22 using your preferred method (nvm, installer, etc.).

## GenKit Functions (genkit_rag_service)

Location: `genkit_rag_service/functions`

Key scripts (see `package.json`):

- `npm run genkit:start` — starts GenKit dev (watches `src/index.ts`).
- `npm run build` — TypeScript build (tsc).
- `npm run serve` — builds and starts Firebase emulators for functions.
- `npm run deploy` — deploys functions to Firebase project.

Secrets / API keys:


1. Add your Google GenAI API key as a Firebase Functions secret (recommended):

```powershell
# Replace <PROJECT_ID> with your Firebase project ID
firebase functions:secrets:set GOOGLE_GENAI_API_KEY --project <PROJECT_ID>
```


1. Run GenKit dev for iterative flow development (optional):

```powershell
cd genkit_rag_service/functions
npm run genkit:start

# This launches genkit in dev/watch mode and runs the TypeScript entry with tsx.
```

1. Deploy to production:

```powershell
cd genkit_rag_service/functions
npm run deploy

# or from genkit_rag_service root:
cd ..\
firebase deploy --only functions --project <PROJECT_ID>
```

Notes:

- If you prefer local `.env` for development, uncomment the dotenv section in `src/index.ts`, create a `.env` file two levels up from `functions` (the code expects `../../.env`), and set a key named `GEMINI_API_KEY` or adjust the variable name to match the source.
- The function uses the GenKit plugin for Google GenAI and sets model `gemini-2.5-flash-image` — ensure your API key has access.

## Flutter Example App (hr_agent_flutter_app)

Location: `hr_agent_flutter_app/`

The Flutter app demonstrates calling the `imageGeneration` callable function. The app includes a `firebase_default_options.dart` file and an Android `google-services.json` already present — verify they match your Firebase project.


Steps to run the Flutter example:

1. Open a terminal and fetch Flutter packages:

```powershell
cd hr_agent_flutter_app
flutter pub get
```

1. If you need to connect the app to your Firebase project, replace the `google-services.json` (Android) and the iOS `GoogleService-Info.plist` if present, and regenerate `firebase_default_options.dart` if you change the project settings.

1. Run the app on an emulator or device:

```powershell
# Android
flutter run -d android

# Windows (web target)
flutter run -d web
```

1. When the app calls the callable function, ensure either:

- the function is deployed to the same Firebase project configured in the app, or
- you are running the Firebase emulator and the app is configured to point to emulated functions (set via Firebase SDK in the app code).

