# SharedHands

SharedHands translates American Sign Language (ASL) in real time using your webcam. Hand landmarks are detected with MediaPipe, and TensorFlow.js models classify them into letters and a few words. All of this runs in the browser. Firebase handles accounts and user profiles.

## Features

- Live webcam ASL recognition (letters A–Z, plus "SORRY" and "THANK YOU" from the transformer model)
- Hand landmark overlay drawn on the video feed
- Account registration and login with Firebase Auth
- User profiles and preferences stored in Firestore

## Tech stack

| Layer    | Tools |
| -------- | ----- |
| Frontend | React 19, React Router, Create React App |
| ML (browser) | MediaPipe Tasks Vision, TensorFlow.js |
| Backend  | Python, Flask, Firebase Admin SDK |
| Data     | Firebase Auth, Cloud Firestore |
| Training | TensorFlow / Keras, scikit-learn, OpenCV |

## Project structure

```
public/            Static assets and the exported TF.js models (asl_model/, asl_transformer_model/)
src/
  pages/           HomePage, LoginPage, RegisterPage, Dashboard
  components/      Shared UI (Navbar)
  context/         Auth and settings React contexts
  hooks/           useASLTranslation: in-browser detection and classification
  firebaseConfig.js  Firebase client setup (reads from environment variables)
artifacts/         Python backend (firebase.py) plus model training and conversion scripts
docs/              Design notes, research, and sprint documentation
```

## Getting started

### Prerequisites

- Node.js 18+ and npm
- Python 3.10 or 3.11 (for the backend and training scripts)
- A Firebase project with Authentication (Email/Password) and Firestore turned on

### 1. Clone and install

```bash
git clone https://github.com/Jopersaud/sharedhands.git
cd sharedhands
npm install
```

### 2. Configure environment variables

```bash
cp .env.example .env
```

Fill in `.env` with your Firebase web app config. You can find it in the Firebase Console under **Project settings → General → Your apps**. Don't commit `.env`; it's already in `.gitignore`.

### 3. Set up the backend (optional)

The Flask server in `artifacts/firebase.py` handles `/register` and `/login`. The React dev server proxies these requests to `http://127.0.0.1:5000`.

1. In the Firebase Console, go to **Project settings → Service accounts** and generate a private key.
2. Save the key as `SharedHandsAdminKey.json` in the repository root. It's gitignored. Never commit it.
3. Install the dependencies and start the server:

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
cd artifacts
python firebase.py
```

### 4. Run the app

```bash
npm start
```

Open [http://localhost:3000](http://localhost:3000) and allow camera access when your browser asks.

## Available scripts

| Command         | Description |
| --------------- | ----------- |
| `npm start`     | Start the development server on port 3000 |
| `npm run build` | Create a production build in `build/` |
| `npm test`      | Run the test runner in watch mode |

## Retraining the models

The training scripts are in `artifacts/`:

- `train_the_model_asl.py`: trains the per-frame letter classifier
- `train_the_transformer.py`: trains the sequence (transformer) model
- `convert_model.py` / `retrain_and_convert.py`: export Keras models to TensorFlow.js in `public/`

For dataset details, see `docs/DataSets.md` and `docs/dataset_extraction.md`.

## Security notes

- Keep secrets such as `.env` and service-account JSON files out of the repo.
- The Firebase web API key is sent to the browser by design. Protect your data with Firebase Security Rules, and restrict the key to your domains in the Google Cloud Console.
