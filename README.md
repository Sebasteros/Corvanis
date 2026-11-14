# Corvanis

A cyberpunk D&D 5e sandbox: a live terminal for players, a DM console for the Dungeon Master, and a free wiki that holds the whole setting.

## Quick links

| What | Link | Who it is for |
| --- | --- | --- |
| Player terminal | https://sebasteros.github.io/Corvanis/ | Players |
| Player wiki | https://sebasteros.github.io/Corvanis/wiki/ | Players and DMs |
| DM console | https://sebasteros.github.io/Corvanis/admin.html | The DM (sign in with the DM account) |
| DM edition of the wiki | https://sebasteros.github.io/Corvanis/dm/ | The DM (password required) |

On a phone, the wiki keeps its header at the top of the screen. Tap the page name on the right to open a menu and jump to any section.

## What is here

- **Player terminal:** `index.html`. Players sign in with their character name and a PIN, then use the apps: Map, Neural-Tag, Augments (skill tree), Bank, Vitalis, Jobs, Contacts, Notes and News.
- **DM console:** `admin.html`. Signed in with the DM's email. Manages the world (cycle, interest, glitches, city levels, news), the job board, every player account, bank accounts, augments and skill points, messages, and broadcasts.
- **Player wiki:** `wiki/`. The full player-facing setting: rules, augments, gear, relics, the world, and factions.
- **DM edition:** `dm/`. The same wiki plus the DM-only pages. Every page is encrypted and opens only with the DM's password.
- **Database:** Firebase Firestore. The rules are kept in the Firebase console (Rules tab).

## Files

| File | What it is |
| --- | --- |
| `index.html` | Player terminal (single file: styles, layout and scripts). |
| `admin.html` | DM console (single file). |
| `map-data.js` | Map geometry shared by the player terminal and the DM console (regions, city levels, places). |
| `wiki/` | Player wiki, built from the Corvanis source pages. |
| `dm/` | Encrypted DM edition. File names are scrambled on purpose; open `dm/` and enter the password. |
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
4. Paste the contents of `firestore.rules` into the Rules tab whenever it changes, and press **Publish**.
5. In GitHub, go to **Settings > Pages**, set the source to the `main` branch, root folder, and save.

## Known limits

- The PIN check happens in the browser. It keeps players out of each other's accounts casually, but anyone who reads the database can see account data. Players cannot change balances or passes; only the DM can.
- The site is free and static (GitHub Pages plus Firebase's free tier).
