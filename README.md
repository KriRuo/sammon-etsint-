# Sammon etsintä

A Finnish take on *Afrikan tähti*. Race across Finland to find the Sampo, the magic mill of the Kalevala, and carry it back to Helsinki or Rovaniemi. For 2–6 players passing one phone around.

This is the **test build** from the design spec: a single-file web app for Chrome on Android. You don't need to install anything or go through an app store.

## Play it

The whole game is in `index.html`. The other files are optional helpers:

| File | Purpose |
| --- | --- |
| `index.html` | The game: board, rules engine, UI and save |
| `manifest.webmanifest`, `icon*.png`, `icon.svg` | "Add to Home screen" as a full-screen app |
| `sw.js` | Offline play once the page has loaded |

**Locally:** open `index.html` in a browser. It works from `file://` too, just without offline caching.

**On a phone:** host the folder on any static host, for example GitHub Pages (Settings → Pages → deploy from branch, root folder). Open the URL in Chrome on Android, then use ⋮ → *Add to Home screen*. After the first load it also works offline.

**Save:** the game saves to browser storage after every action, so closing the tab and opening it again resumes the game.

## What's implemented

The rules engine enforces everything in the spec:

- **Board:** 24 token cities plus Helsinki, Rovaniemi and Korvatunturi, joined by road steps, ferry lanes and rail links. Mariehamn and Hailuoto can only be reached by ferry.
- **Road:** roll and move exactly that many steps without doubling back, or stop early at any city. Reachable spaces glow; tap one to move.
- **Ferry (€100):** board at a harbour, roll for distance, and stop at the first harbour. A long crossing can leave you at sea until your next turn. Players with less than €100 sail 2 steps free.
- **Train (€300):** jump directly between two linked stations (🚆).
- **Snowmobile (€200):** only in Lapland, north of the dotted line. Roll and double it.
- **Tokens:** when you land on a city with a hidden token, you choose: pay €100 to flip it, or carry on. On later turns you can roll 4–6 to flip it free. The token mix follows the spec (24 in total).
- **Special spaces:** blizzard ❄️, Baltic storm 🌊 (2 sea steps), mosquito swamp 🦟 (Sodankylä), Lemmenjoki gold fields ✨ (Inari), Korvatunturi 🎅, sauna ♨️, midnight sun ☀️ and reindeer crossing 🦌.
- **Fragment rule:** a fragment flipped before the Sampo is found is discarded. One flipped afterwards is a winning token.
- **Screens:** setup (players, names, colours, start city, starting player) → board with pawns, dice, money and turn log → token pop-up → win screen. Rules are always one tap away (📜). 🔍 zooms the map.

## Decisions on the open questions (easy to change)

- **Language:** English, Finnish and Swedish. Switch with the EN/FI/SV selector on the setup screen, or tap the language button in the game's top bar to cycle through them. The first visit follows the phone's language, and the choice is remembered. In Swedish, cities use their Swedish names (Helsingfors, Åbo, Uleåborg…). Switching mid-game also retranslates the turn log and any open pop-up. All text lives in the `STR` table in `index.html`.
- **Name:** *Sammon etsintä*.
- **Finish cities:** Helsinki and Rovaniemi, as in the spec.
- **Snowmobile:** kept. It makes Lapland worth the trip.
- **Web app vs APK:** web app first. Once the rules feel right, it can be wrapped as an APK (for example with a TWA or Capacitor).

## Rulings the spec left open

- **When special spaces trigger:** only when you *end* a move on them. The one exception is a reindeer crossing, which stops you even when you are passing through.
- **Escaping a blizzard or storm:** when you roll the 1 or 2 you need, you escape and move that many steps straight away.
- **Sauna:** the free flip is saved until you use it at the next hidden token.
- **Hiisi:** takes half your money, rounded down.
- **Flipping later:** if you left a token and flip it on a later turn (by paying or trying your luck), that uses up the turn.
- **24th token city:** the spec's list gives 23 cities (21 mainland plus 2 islands), so **Kokkola** was added to make 24.

Board layout, step counts and special-space positions are all in the data tables at the top of the `<script>` in `index.html` (`CITY_DEFS`, `ROAD_DEFS`, `SEA_DEFS`, `RAIL_DEFS`, `TOKENS`). Tune them there after playtesting.

## Testing

Add `?fast` to the URL to skip dice and pawn animations. This is useful for automated playthroughs.
