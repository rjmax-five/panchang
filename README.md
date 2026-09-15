# पंचांग — offline Hindu calendar PWA (Hindi by default, English in settings)

Files:
- `index.html` — the whole app (astronomy engine + UI, no dependencies)
- `manifest.webmanifest`, `sw.js`, `icon*.png`, `icon.svg` — what makes it installable and offline

## Run it locally (quick look)
Open `index.html` in any browser. Everything works except the install prompt, which needs https.

## iPhone / iPad
Apple does not allow installing an app file directly, so the web-app route is the one to use:
1. Host the files (see the GitHub Pages steps below) and open the address in **Safari** on the iPhone or iPad.
2. Tap the **Share** button → **Add to Home Screen** → **Add**.
3. A पंचांग icon appears on the home screen. It opens full-screen, without Safari's bars, and works offline.
The iPad gets a wider, full-page layout automatically.

## Put it on your phone (once, free)
1. Create a GitHub repo (e.g. `panchang`) and upload these files to its root.
2. Settings → Pages → Source: *Deploy from a branch* → `main` / `(root)` → Save.
3. Open `https://<you>.github.io/panchang/` in Chrome on your Android phone.
4. Chrome menu → *Add to Home screen* (or *Install app*). It now opens full-screen and works offline.

Any later edit: push to the repo, reopen the app twice (the service worker updates on the second load).
If you change the app and it seems stuck on an old version, bump `CACHE = 'panchang-v1'` in `sw.js` to `v2`.

## How dates are computed
- Sun/Moon longitudes: Meeus *Astronomical Algorithms* (ch. 25 & 47). New/full moons check within ~1 minute of published tables.
- Ayanamsa: Lahiri (Chitrapaksha).
- Tithi/nakshatra/yoga/karana are taken at local sunrise; end times are found by root-finding.
- Lunar month: amanta by default, named for the sankranti it contains; adhik masa detected automatically.
- Festivals: rule table in `FEST` (amanta masa + tithi). Some use a different deciding moment — noon (Ganesh Chaturthi, Rama Navami), afternoon (Dussehra), evening (Diwali, Dhanteras, Holika Dahan), midnight (Janmashtami, Shivaratri). A tithi that starts and ends between two sunrises (kshaya) is still assigned to a day.
- Sankranti festivals (Makar Sankranti, Baisakhi) are placed on the day the Sun enters the rashi.

## Adding a festival
Add a line to `FEST` in `index.html`:
```js
{ m: 7, t: 10, name: 'Dev Uthani Ekadashi', dev: 'देवउठनी एकादशी', kind: 'minor' },
```
`m` = amanta month (0 Chaitra … 11 Phalguna), `t` = tithi 0–29 (0–14 Shukla, 15–29 Krishna), `kind` = `major` | `minor`, optional `anchor` = `noon` | `aparahna` | `pradosh` | `midnight`.
