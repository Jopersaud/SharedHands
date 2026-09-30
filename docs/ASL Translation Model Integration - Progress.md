
# ASL Translation Model Integration — Progress Documentation

**Task:** Continue integrating the ASL translation model into the Flask backend in collaboration with Dillon  
**Status:** In Progress  
**Role:** Backend Developer



## Progress Summary

## Screenshots

![Task Overview](/docs/images/Screenshot%202026-03-25%20190500.png) _Task details and acceptance criteria_

![Progress Update](/docs/images/Screenshot%202026-03-25%20191041.png) _Progress update_

### Environment Setup (Completed)

-   Cloned the repository from `https://github.com/Jopersaud/sharedhands.git` and checked out the `front+backend` branch
-   Resolved Windows Long Path support issue required for TensorFlow installation
-   Successfully installed all required Python dependencies:
    -   `opencv-python`
    -   `mediapipe==0.10.9`
    -   `numpy==1.26.4`
    -   `tensorflow==2.13.0`
    -   `keras`
    -   `scikit-learn`
    -   `pandas`
    -   `firebase-admin`
    -   `flask`, `flask-cors`, `flask-socketio`
    -   `pillow`
-   Resolved protobuf version conflict — pinned to `protobuf==3.20.3` for MediaPipe compatibility

### ASL Model Testing (Completed)

-   Successfully ran `ASL_Version 1.0.py` standalone to verify MediaPipe hand landmark detection and model inference pipeline
-   Confirmed hand landmark drawing and letter prediction logic works via webcam feed
-   Verified smoothing window and confidence threshold logic functions correctly

### Flask Backend Integration (In Progress)

-   Reviewed and set up `firebase.py` — the main Flask + SocketIO backend server
-   Backend exposes a `video_frame` WebSocket event that:
    -   Decodes incoming base64 image frames from the frontend
    -   Processes frames through MediaPipe Hands for landmark detection
    -   Runs landmark data through the Keras ASL model for letter prediction
    -   Returns predicted letter, confidence score, and annotated frame back to the frontend
-   Firebase Admin SDK initialized for Firestore user management
-   `/register` and `/login` REST endpoints implemented and functional

### Frontend–Backend Connectivity (Almost Completed)

-   React frontend (`npm start`) configured and running on `localhost:3000`
-   Resolved multiple Windows-specific frontend issues:
    -   `allowedHosts` schema error fixed via `HOST=localhost` in `.env`
    -   OpenSSL legacy provider flag added via `cross-env` in `package.json`
    -   Dependency conflicts resolved with `--legacy-peer-deps`
-   Cross-device development setup established:
    -   Flask backend running on Windows PC
    -   React frontend running on MacBook
    -   Devices connected via **Tailscale VPN** (`100.x.x.x`) to bypass AP isolation
    -   Flask server configured to bind on `host='0.0.0.0'` to accept remote connections

----------

## Known Issues / Blockers

Issue Status

`protobuf` version conflicts with TensorFlow and MediaPipe Resolved — pinned to `3.20.3`

`asl_model.keras` and `asl_landmarks.csv` must be present in `artifacts/` Requires model files from teammate

React frontend dependency conflicts on Windows Resolved

OpenSSL error with Node.js v24+ Resolved via `cross-env`

----------

## Next Steps

-   [ ] Obtain `asl_model.keras` and `asl_landmarks.csv` from teammate and place in `artifacts/`
-   [ ] Test full end-to-end pipeline: React frontend → WebSocket → Flask backend → ASL model → response
-   [ ] Fix any remaining video feed / landmarking errors identified during live testing
-   [ ] Collaborate with Dillon to verify React SocketIO client is correctly sending and receiving frames
-   [ ] Validate prediction accuracy and confidence thresholds in real-time use

----------

## Dependencies & Versions

Package Version

Python 3.11

tensorflow 2.13.0

mediapipe 0.10.9

protobuf 3.20.3

numpy 1.26.4

keras (bundled with TF 2.13)

flask latest

flask-socketio latest

firebase-admin latest

Node.js 24.x

react-scripts 5.x
