# Sajed Fantasy

A full Fantasy Premier League companion site — players, teams, fixtures, live
gameweek points, dream team and a player comparison tool — built with plain
HTML/CSS/JS and the public FPL API. No build step, no backend, no paid
services.

## Run it locally
Just open `index.html` in a browser, or serve the folder:
```
python3 -m http.server 8000
```

## Deploy on GitHub Pages (free)
1. Push this folder to a GitHub repo.
2. Repo **Settings → Pages → Build and deployment → Deploy from a branch**.
3. Branch: `main`, folder: `/root`. Save.
4. Your site is live at `https://<username>.github.io/<repo>/` in ~1 minute.

## About the API / CORS
The FPL API (`fantasy.premierleague.com/api/...`) doesn't send CORS headers,
so a static site calling it directly from the browser gets blocked. `app.js`
tries a direct call first, then falls back to a couple of free public CORS
relays (`allorigins.win`, `corsproxy.io`, `thingproxy`). This needs no key and
no paid plan, but free relays can be slow or rate-limited — if data won't
load, wait a minute and refresh. For a production Telegram Mini App, replace
the relay with your own tiny proxy (a few lines on Cloudflare Workers or a
Vercel serverless function) that just forwards the request and adds a CORS
header — this removes the dependency on third-party relays entirely.

## "My Team" — why not Google sign-in?
FPL has no public "Sign in with Google" and no OAuth for third-party sites —
the only real login lives on the official FPL site/app. What FPL *does*
expose publicly, with no password at all, is a manager's numeric **Team ID**
(the number in `fantasy.premierleague.com/entry/<id>/...` when you're logged
into your own account). Anyone's squad and history can be viewed with just
that number — it's already public the same way your overall rank is public —
so the "My Team" tab just asks for that one number, saves it in the browser's
`localStorage`, and pulls the manager info, gameweek history and squad picks
from the public `/entry/{id}/` endpoints. Nothing is ever sent anywhere
except straight to the FPL API (via the CORS relay).

## Turning this into a Telegram Mini App later
1. Add `<script src="https://telegram.org/js/telegram-web-app.js"></script>`
   to `index.html`.
2. Call `Telegram.WebApp.ready()` and `Telegram.WebApp.expand()` at the top
   of `app.js`.
3. Register the site URL with @BotFather as your bot's Web App / menu button.
   No other changes are required — the same GitHub Pages URL works as-is.

## Structure
- `index.html` — layout, all views, design tokens (CSS variables)
- `app.js` — API calls, state, rendering, sorting/filtering, countdown
