# Pip-Boy Jetpack Run

A browser game inspired by Flappy Bird: choose a pilot, boost through the wasteland, dodge radiation barrels, and collect Nuka-Cola caps.

Open `index.html` in a browser to play.

## Daily leaderboard setup

The game is ready for a Firebase Firestore daily top-5 scoreboard. To turn it on, create a Firebase web app, enable Firestore, then paste your web config values into `FIREBASE_CONFIG` in `index.html`.

Suggested Firestore rules:

```txt
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /leaderboards/{day}/scores/{scoreId} {
      allow read: if true;
      allow create: if request.resource.data.initials is string
        && request.resource.data.initials.size() > 0
        && request.resource.data.initials.size() <= 10
        && request.resource.data.score is number
        && request.resource.data.score >= 0
        && request.resource.data.characterId is string
        && request.resource.data.characterName is string
        && request.resource.data.createdAt is timestamp;
      allow update, delete: if false;
    }
  }
}
```
