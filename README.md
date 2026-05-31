# Duck Duck Nuke

by Wulfzx.Underground

A browser game inspired by Flappy Bird: choose a pilot, boost through the wasteland, dodge WZX barrels, and collect red WZX caps.

Open `index.html` in a browser to play.

## Daily leaderboard

The game uses Firebase Firestore for a public daily top-5 scoreboard. Scores are grouped by UTC date under `leaderboards/{YYYY-MM-DD}/scores`, so a fresh board starts each day. Each submission is stored as a create-only score entry, and the game displays only the best score per initials.

The project is configured for the Firebase project `pip-boy-jetpack-run`. Firestore should stay on the Spark plan with these rules:

```txt
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /leaderboards/{day}/scores/{scoreId} {
      allow read: if true;
      allow create: if request.resource.data.keys().hasOnly(['initials', 'score', 'characterId', 'characterName', 'createdAt'])
        && request.resource.data.initials is string
        && request.resource.data.initials.size() > 0
        && request.resource.data.initials.size() <= 10
        && request.resource.data.score is number
        && request.resource.data.score >= 0
        && request.resource.data.characterId is string
        && request.resource.data.characterId.size() > 0
        && request.resource.data.characterName is string
        && request.resource.data.characterName.size() > 0
        && request.resource.data.createdAt == request.time;
      allow update, delete: if false;
    }
  }
}
```
