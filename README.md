# Corvanis

A cyberpunk D&D 5e sandbox: a live terminal for players and a DM console for the Game Master.

- **Player terminal:** `index.html`. Players sign in with their character name and a PIN, then use the apps: Map, Neural-Tag, Wallet, Messages, Jobs, Contacts, Notes and News.
- **DM console:** `admin.html`. Signed in with the DM's email. Manages the world (cycle, interest, glitches, city levels, news), the job board, every player account (profile, wallet, passes, phone contents, messages), secrets, and broadcasts.
- **Database:** Firebase Firestore. The rules are in `firestore.rules`.

Live site: https://sebasteros.github.io/Corvanis/
DM console: https://sebasteros.github.io/Corvanis/admin.html

## Files

| File | What it is |
| --- | --- |
| `index.html` | Player terminal (single file: styles, layout and scripts). |
| `admin.html` | DM console (single file). |
| `map-data.js` | Map geometry shared by the player terminal and the DM console (regions, city levels, places). |
| `firestore.rules` | Database security rules. Paste into Firebase, then Publish. |
| `README.md` | This file. |

## Data layout (Firestore)

- `state/main`: world state (cycle, levels, news, glitch time, job board).
- `accounts/{id}`: one document per player. `id` is the character name slugged (for example `Test Player` becomes `test-player`). Holds the PIN salt and hash, wallet, debt, passes, contacts, notes, portrait and log.
- `accounts/{id}/messages/*`: messages sent to that player's phone.
- `secrets/{id}`: DM-only notes about a player. Players can never read these.
- `players/{id}`: presence (last seen), used for the online list.

## Keeping the data safe

- Every write merges into the existing document. Updates add or change only the fields they name.
- Nothing is deleted automatically. Deletes happen only when the DM presses DELETE and confirms.
- Field names stay stable. New fields get default values, so older accounts keep working.
- Use **DOWNLOAD BACKUP** in the DM console to save a JSON copy of accounts, messages, secrets and the world state.

## Setup

1. Create a Firebase project and add a web app. Copy its config into the `firebaseConfig` object in `index.html` and `admin.html`.
2. In Firebase, enable **Authentication**, with the Email/Password and Anonymous sign-in providers. Create the DM's email and password account.
3. Create a **Firestore** database. Open the **Rules** tab, paste the contents of `firestore.rules`, and press **Publish**. Make sure the DM email in the rules matches the DM's login.
4. In GitHub, go to **Settings > Pages**, set the source to the `main` branch, root folder, and save.

## Known limits

- The PIN check happens in the browser. It keeps players out of each other's accounts casually, but anyone who reads the database can see account data. Players cannot change balances or passes; only the DM can.
- The site is free and static (GitHub Pages plus Firebase's free tier).
