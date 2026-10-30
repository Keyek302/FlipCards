# Flip Deck

A family flashcard and quiz app that installs on a phone like a regular app and works offline.
It is one web page with no build step and no accounts, and it needs no server code.

**What's inside**

- **Flashcards for every age:** Little learners (3–5), Grade school (6–9), Big kids (10–13), and Adults (GED prep).
  Picture and spoken versions help kids who can't read yet.
- **Two ways to play:** flip cards, or multiple choice with believable wrong answers.
- **Spaced repetition:** missed cards come back later; mastered decks can be practiced again.
- **Explanations:** a "Let's learn it" popup when you miss a card.
- **Shuffle all:** mix every deck in an age section into one round.
- **GED Arena:** quiz show (lives, streak multiplier, lifelines, "bet on it"), head-to-head, and a timed practice test.
- **Levels and XP:** XP for every correct answer, a bonus for finishing a deck, and badges.
- **For Josh:** endless Solve for x and Calculus practice with a calculator keypad, hints, and lessons.

## Files

| File | What it does |
| --- | --- |
| `index.html` | The whole app (HTML, CSS, JavaScript, and all the cards) |
| `manifest.webmanifest` | Tells phones this is installable: name, icons, colors |
| `sw.js` | Service worker: caches the app so it works offline |
| `icons/` | App icons (192, 512, maskable, Apple touch icon, favicon, SVG) |
| `.nojekyll` | Tells GitHub Pages to serve the files as they are |

## Run it on your computer

```bash
cd flip-deck
python3 -m http.server 8080
# open http://localhost:8080
```

(Opening `index.html` by double-clicking works for the app itself, but the install and
offline features need it served over `http://localhost` or `https://`.)

## Publish with GitHub Pages (free)

1. Create a new GitHub repo and upload everything in this folder (keep `index.html` at the top level).
2. In the repo go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, pick `main` and `/ (root)`, then **Save**.
4. After a minute your app is live at `https://YOUR-USERNAME.github.io/YOUR-REPO/`.

All the file paths are relative, so it works from a repo subfolder like that with no changes.

## Install on a phone

- **iPhone / iPad (Safari):** open the link, tap **Share → Add to Home Screen**.
- **Android (Chrome):** open the link, tap **⋮ → Install app** (or **Add to Home screen**).

After the first load it works with no internet. It opens full screen with the Flip Deck icon.

## Updating the app

1. Edit `index.html` (or any file).
2. Open `sw.js` and change `CACHE_VERSION` (for example `flipdeck-v1` → `flipdeck-v2`).
3. Commit and push. Phones pick up the new version the next time the app is opened with internet
   (you may need to close and reopen it once).

If you skip step 2, phones can keep showing the old version.

## Where the data lives

Progress, XP, badges, custom decks, and settings are saved in the browser's local storage on each device
(key `flipdeck-v2`). That means:

- Each phone keeps its own progress, and nothing is sent anywhere.
- Clearing the browser's site data resets progress. Uninstalling the app can too.
- Progress doesn't sync between phones. Adding a sync feature would need a backend.

## Adding or changing cards

The cards are near the top of the `<script>` in `index.html`:

- `D` has the main decks: `[group, emoji, name, color, [[question, answer, level], ...]]`.
  Groups are `a` (3–5), `b` (6–9), `c` (10–13), `g` (adult GED), and `x` (hidden).
  Level is optional: `1` easy, `2` medium (default), `3` hard.
- `X`, `Y`, `Z`, `Z2`, `Z3` add more cards to a deck by deck name. They are added to the end so saved progress stays attached.
- `H` holds the "Let's learn it" explanations, keyed by the question text.
- `W` holds hand-picked wrong answers for multiple choice, keyed by the question text.

Always add new cards to the **end** of a deck. Progress is saved by card position, so inserting in the middle would shift it.

## Turning it into a real App Store / Google Play app (optional)

The installable web app above is the easiest route. If you later want store listings, wrap it with
[Capacitor](https://capacitorjs.com/):

```bash
npm init -y
npm i @capacitor/core @capacitor/cli
npx cap init "Flip Deck" com.yourname.flipdeck --web-dir=.
npm i @capacitor/android @capacitor/ios
npx cap add android && npx cap add ios
npx cap sync
npx cap open android   # or: npx cap open ios
```

You'll need Android Studio for Google Play, and a Mac with Xcode plus an Apple Developer account for the App Store.

## Notes

- The "Read to me" voice uses the phone's built-in speech, so the voices depend on the device.
- The GED practice test is only a practice estimate, not an official GED score.
- The coffee tab and footer link to `buymeacoffee.com/keyek` and `keyek302.github.io`. Search `index.html` for those to change them.
