# FYV (Find Your Vibe)

Random video chat for college students. React, Express, PeerJS and Firestore.

## What it does

- Register and log in with email and password. Passwords are hashed with bcrypt.
- Pick interests at sign-up. They are stored in Firestore.
- Join the lobby and get matched with another waiting user, then talk over WebRTC video and audio (PeerJS) with a text chat beside it.
- Skip to the next person, leave, or report someone. After more than four reports a user is banned for 15 minutes.
- An ID scan page reads a QR code from an uploaded ID card image on the server.

## How it works

Firestore holds the state: `users`, `connection` (who is waiting or matched), sessions and `reports`. The backend does matchmaking over REST (`/api/peer/matchmake`). It picks a random waiting user other than the caller, marks both as matched and creates a session. It skips a pair that was already matched in the last 10 seconds.

Media goes peer to peer. Each client registers a PeerJS id with the public PeerJS cloud server (`0.peerjs.com`), sends it to the backend with `joinroom`, and the matched pair call each other. Google STUN servers handle NAT traversal. Chat messages go through Socket.IO and are broadcast to every connected client.

## Stack

- Frontend: React 19, React Router, PeerJS, Socket.IO client, react-webcam, built with CRA via craco
- Backend: Node.js, Express, Socket.IO, Firebase Admin / Firestore, bcryptjs, multer, jimp and qrcode-reader
- Database: Firestore

## Run it locally

```bash
# backend (port 5000)
cd Backend
npm install
npm run dev

# frontend (port 3000)
cd Frontend
npm install
npm start
```

Backend env vars: `PORT`, `FRONTEND_URL`, `JWT_SECRET`, `FIREBASE_PROJECT_ID`, `FIREBASE_CLIENT_EMAIL`, `FIREBASE_PRIVATE_KEY`, `FIREBASE_SERVICE_ACCOUNT_JSON`

Frontend env var: `REACT_APP_API_URL` (defaults to `http://localhost:5000` in some pages, so set it)

There is no `.env.example`. You need your own Firebase project with Firestore.

## Notes

College project, built with [Alwin Johnson](https://github.com/Alwin-Johnson), [Anna Sara Joji](https://github.com/asj327) and [Drupad Praveen](https://github.com/drupadpraveen).

Matching is random. Interests are saved but not used for matching yet, and the "college authentication" is the ID scan page, not a verified email check. No live deployment is linked.
