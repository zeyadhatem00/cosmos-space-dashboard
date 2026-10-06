# COSMOS — Space Dashboard

COSMOS is a responsive, browser-only space dashboard built with plain HTML, CSS, and JavaScript. It brings astronomy imagery, upcoming launch data, and solar-system reference data into one dark, mobile-friendly interface.

## What it includes

- **Today in Space** — loads NASA’s Astronomy Picture of the Day, including the title, explanation, date, media type, copyright, and a full-resolution link when the API response provides one.
- **Date browsing** — choose an APOD date from the date picker and load that entry; the API’s supported lower bound is reflected in the UI.
- **Upcoming launches** — requests the next 10 launches from The Space Devs and renders one featured launch plus launch cards with provider, vehicle, UTC time, location, status, and mission details.
- **Planet explorer** — displays eight planet cards, then fetches detailed planetary data from a Solar System OpenData proxy when a planet is selected.
- **Reference content** — includes a static planet comparison table and locally stored planet illustrations for the initial interface.
- **Responsive navigation** — switches between Today in Space, Launches, and Planets through a sidebar that collapses on smaller screens.

## Technology

- Semantic HTML entry point: [`index.html`](index.html)
- Vanilla JavaScript for navigation, rendering, and API requests: [`js/main.js`](js/main.js)
- Checked-in Tailwind CSS v4.1.17 output plus project-specific theme variables: [`css/style.css`](css/style.css)
- Font Awesome 6.4.0 and Google Fonts (Space Grotesk and Inter) loaded from CDNs
- Local PNG/WebP illustrations and fallback imagery in [`images/`](images/)
- No framework, package manifest, build tool, or dependency lockfile is present in this snapshot.

## Requirements

- A modern browser with JavaScript enabled
- Internet access from the browser for the NASA APOD, Solar System OpenData, and The Space Devs requests, plus the font/icon CDNs
- Python 3 (or another static HTTP server) for the simplest local preview

There are currently no documented environment variables. The NASA APOD request uses a client-side key embedded in `js/main.js`; this README intentionally does not reproduce it. Treat that key as exposed browser configuration rather than a server secret, and move API access behind a server-side proxy or otherwise rotate/configure it before production use.

## Run locally

From the repository root:

```bash
python3 -m http.server 8000
```

Open <http://localhost:8000> in a browser. Stop the server with `Ctrl+C`.

This is a static site, so there is no install step and no build command. A local HTTP server is recommended while developing because the page loads JSON and media from external services.

## Data sources and runtime behavior

The browser calls these services directly from [`js/main.js`](js/main.js):

- [NASA APOD API](https://api.nasa.gov/planetary/apod) for today’s and selected-date astronomy media.
- [Solar System OpenData proxy](https://solar-system-opendata-proxy.vercel.app/api/planets) for planet metadata and detail views.
- [The Space Devs Launch Library](https://lldev.thespacedevs.com/2.3.0/launches/upcoming/?limit=10) for the upcoming-launch list.

The launch cards use the checked-in [`images/launch-placeholder.png`](images/launch-placeholder.png) when an API item has no image or an image fails to load. APOD errors display an in-page failure message, but the launch and planet requests do not provide an equivalent user-facing error state; an unavailable endpoint can therefore leave those sections empty or incomplete.

Some controls are presentation-only in the current implementation. The favorite buttons, launch “Details”/“View Full Details” buttons, notification button, and planet “Learn More” button do not have actions wired in `js/main.js`. The APOD date picker, navigation, mobile sidebar toggle, and full-resolution viewer are wired browser interactions.

## Project layout

```text
.
├── index.html                         # Single-page dashboard markup and static planet table
├── js/main.js                         # Navigation, API fetches, rendering, and interactions
├── css/style.css                      # Checked-in Tailwind-generated CSS and theme rules
├── images/                            # Planet, favicon, and launch fallback assets
└── .github/workflows/static.yml       # GitHub Pages deployment workflow
```

## Deployment

The repository includes a GitHub Actions workflow that uploads the repository root to GitHub Pages when changes are pushed to `main`, and also supports manual dispatch. The repository metadata does not provide a verified public Pages URL, so no live-demo link is claimed here.

## Contributing locally

Keep the app dependency-free unless the project gains an explicit package manifest. When changing API-backed UI, update the corresponding loading/error behavior as well as the markup, and avoid committing credentials or other private configuration to client-side files.
