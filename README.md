# Conversational Voice Capture app

## Overview

The Conversational Voice Capture app is a general purpose web app for connecting participants to engage in realtime conversations based on dynamically generated prompts. This program not only comes with a user-facing web interface, but it also comes equipped with an admin dashboard for application managers to monitor and ensure everything is running smoothly on their deployment. This general-purpose app is ideal in scenarios such as remote meetings, virtual events, podcasts, and collaborative discussions. Participants can seamlessly connect, converse, and capture these interactions with ease.


## Wayfinding

- `/webapp/src` - contains the user-facing Vite React app
- `/admin/src` - contains a Vite React app for an admin dashboard to monitor overall app state
- `/admin/functions` - contains serverless backend code using Firebase Cloud Functions that facilitates user lobbies and matching. See `admin/functions/src/index.ts` for full code.

## Local Development

The app can be run using either an actual Firebase account setup or the Local Emulator Suite.

For local development, the Firebase Local Emulator Suite suite is ideal. You will save a lot of time here, as deploying cloud functions takes a while.

### Prerequisites:

Before getting started, please make sure you have these tools installed/setup:

###### 1. Node.js (Emulator Suite)

###### 2. Java JDK (Emulator Suite)

###### 3. Firebase Realtime Database

###### 4. Firebase Cloud Functions

###### 5. Firebase Auth

###### 6. Firebase CLI:

* The [Firebase CLI](https://firebase.google.com/docs/cli) is needed to deploy Firebase Functions and run the local emulator. Run `firebase login`
* Your Firebase configuration information should be added to `webapp/src/firebase.tx`, `admin/src/firebase.tsx`, and `admin/.firebaserc`

###### 7. Daily.co API Key:

* A Daily.co API key is required for WebRTC room creation. Create a file at `/admin/functions/.env` file with `DAILY_API_KEY=<KEY>`.
* **FOR STORING AUDIO TRACKS:** account to be updated to the pay-as-you-go plan

###### 8. Amazon S3 Bucket

* An S3 bucket is required for deploying the webapp + admin interface, too. By default we support S3 however you can use any static webhost.
  * *Note: you can use the [Firebase Local Emulator Suite](https://firebase.google.com/docs/emulator-suite/install_and_configure) to test locally as well*
* **FOR STORING AUDIO TRACKS:** An S3 bucket is required to deliver downloaded tracks to. [(see instructions)](https://docs.daily.co/guides/products/live-streaming-recording/storing-recordings-in-a-custom-s3-bucket)


### Configure the web interfaces to use the local endpoint

By default, when running locally using `npm run dev`, the app will always use the Local Emulator Suite. There may be cases where you'd like to change this behavior to use a hosted environment instead.

**Option 1:**
Use the provided `npm run dev:remote` script (note the `:remote` part) when launching either the admin interface or user-facing app.

**Option 2:**
Use the `VITE_USE_REMOTE=true` flag, e.g. `VITE_USE_REMOTE=true npm run dev` when running the front-end build.

**Option 3:**
Set the `USE_LOCAL_EMULATOR` flag manually in:

- `/admin/src/firebase.tsx` for the admin interface
- `/webapp/src/firebase.tsx` for the user-facing interface

### Run the Local Emulator Suite

```
cd admin/functions
npm install
npm run em
```

### Launch the admin interface for monitoring

```
cd admin/src
npm install
npm run dev
```

### Launch the user-facing webapp

```
cd webapp
npm install
npm run dev
```

---

## Deploying

Firebase Cloud Functions

_Requires the [env var](https://firebase.google.com/docs/functions/config-env) `DAILY_API_KEY` to be set (e.g. via `admin/functions/.env`)_

```
cd admin/functions
npm install
echo "DAILY_API_KEY=$SECRET_KEY" >> .env
npm run deploy
```

The webapp interface can be deployed via the [Deploy to AWS GH Action](https://github.com/fairinternal/openvox/actions/workflows/deploy.yml).

Select `Run workflow` and the branch you'd like to deploy.

## TODO:

- Figure out a strategy for deploying the admin interface. What security model should we use here?
- Do we need to secure the Firebase Realtime Database with certain rules. Should the rules be checked into this repo?
- Decouple front-end components so that they are testable without access to a backend
- Integration with scheduling/calendar tool?
