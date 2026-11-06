# Matematik I seminars: live mode

`index.html` holds the 11 seminars (one "Träff" per meeting, like the lectures in the MM5023 companion), a section "Extra" with two standalone parts ("Att skriva matte", which goes with Träff 1, and "Att läsa matte"), and a section "Tentaförberedelse" with two exam-prep sessions: "Tentaträning 1" (Analys del 1 and Diskret matematik) and "Tentaträning 2" (Analys del 2 and Linjär algebra). Exam problems have a think–pair–share timer, hints students open one at a time, an answer box, and a step-by-step solution that appears when you press **Visa lösningen**. Every question is interactive: quick polls, true/false and arrow tables, free-text answers, sketches drawn on the phone, and proof puzzles. Live mode works peer-to-peer, as in MM5023: no server, no accounts.

The **Schema** button in the top bar (and on the start page) opens the HT26 overview schedule. The current week is highlighted, and each Träff links to its lecture. To change the schedule, edit the `SCHED` list at the end of `web/app.js`.

## One-time setup: GitHub Pages
Put `index.html` in its own folder of your GitHub Pages repository, for example `seminarier/index.html` next to the exam-prep page. The address is then `https://<your-username>.github.io/<repo>/seminarier/`.

## In the seminar
1. On the lecture computer, open the address with `?present` at the end, for example `…/seminarier/?present#T03`. The QR code appears. Students scan it, or type the 4-letter code on the start page.
2. The page always starts in **Eget tempo**. The dark bar at the bottom has two modes:
   - **Följ mig** (follow me): students' screens follow your slides, as in MM5023. Use **Öppna frågan** on each question to let them answer.
   - **Eget tempo** (own pace): students move freely through the träff and every question is open. Use this while you give individual feedback on homework.
3. While they work in own pace:
   - **Översikt** (overview) shows how many students are on each slide and how many answers each question has. Click a row to go there.
   - Students can answer even before their phone has connected: the answers are sent when the connection works. Students can press **Be om hjälp** (ask for help) and optionally type where they sit. The request appears in the bar ("1 vill ha hjälp") and in the overview; press **Klar** when you have helped them.
4. Wrap-up together: switch back to **Följ mig**. Every phone jumps to your slide. On each question:
   - **Visa klassens svar** (show the class's answers) shows the vote bars, the list of written answers, the gallery of sketches, or how many put the proof in the right order. Answers are hidden until you press it, so the projector doesn't show them while students are still working.
   - **Visa rätt svar / Visa förslag till svar** (show the right answer / a suggested answer) reveals the answer, with comments, on every phone. Students then also see the class distribution.
   - **Nollställ** (reset) clears that question.
5. **Översikt → Ladda ner alla svar (CSV)** saves every answer, for example to see which topics need repeating.

Answers are anonymous: one per device, and a student can change their answer until you reveal it. Your browser keeps the answers if you reload the page. If a phone loses its connection, its answers are sent again when it reconnects. The join code stays the same between meetings; **Visa QR → Ny kod** makes a new one.

Without `?present` or `?join`, the page works on its own for practice at home: answers are saved in the browser and feedback comes right away.

## Timers
- Every activity box has a countdown set to its suggested time, and exam problems have the Tänk / Par / Dela timer. When you press **Starta** on your screen, the countdown also appears in the bar at the bottom of every student's phone. Phones vibrate when time is up (on phones that support it).
- **⏱ Timer** in the dark bar opens a free stopwatch: choose 1, 2, 3, 5, 10 or 15 minutes, pause, add 30 seconds, or stop. It keeps running when you change slides and is also shown on all phones.
- **Stoppa**, or ↺ on a slide timer, removes the countdown from the phones.

## How it connects
- The free PeerJS server finds the phones and connects them to your browser. After that, answers travel directly between the phones and your browser.
- Restrictive networks such as eduroam often block those direct connections. The traffic then goes through a **TURN relay** on ports 80/443, which eduroam allows.
- The relay is set in `turn.json`, which must sit **next to `index.html`** in the GitHub repository. It is the same relay account as in MM5023 (Metered, free plan with 20 GB per month; a seminar uses a few MB), so no new account or key is needed. Both courses share the quota.
- Without `turn.json`, the page falls back to the public Open Relay service, which is not guaranteed to work.
- Your bar at the bottom shows which relay is in use: `redo · relä: egen TURN (turn.json)` or `redo · relä: publik Open Relay (ingen egen TURN)`.
- If you ever make a new key in the Metered dashboard, update `turn.json` in both repositories (MM5023 and the seminars).

## Testing that eduroam works
- Add `&relay` to a join link, e.g. `…/seminarier/?join=CODE&relay`. This forces the connection through the relay, as on the most restrictive networks. If that phone connects, the relay works.
- If a phone cannot connect within about 30 seconds, it shows "når inte seminariet från det här nätet: prova mobildata eller stäng av VPN". Students can still answer; the answers are sent once the phone connects. Mobile data always works as a fallback.

## Editing
`seminarier-kallkod.zip` contains the source. The content of each träff is in `content/T01.py` … `T15.py` (T12–T13 are Extra, T14–T15 are Tentaträning), in the same format as the MM5023 slides. `python3 build.py` rebuilds `out/index.html`. It needs `katex`, `peerjs` and `qrcode-generator` from npm; set the `NPM` environment variable to their `node_modules` folder.
