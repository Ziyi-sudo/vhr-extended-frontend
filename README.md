# VHR Extended — Frontend

Vue 2 single-page application for an HR-management system, built as coursework for a
software engineering course at China University of Petroleum (Beijing).

This repository contains the frontend only. The backend lives in
[vhr-extended-backend](https://github.com/Ziyi-sudo/vhr-extended-backend) and must be
running on port `8081`.

## Attribution

The baseline system is the open-source [VHR](https://github.com/lenve/vhr) project.
The coursework additions on top of that baseline are the 2D and 3D map views and the
linked interaction between them.

## Stack

Vue 2 · Vuex · Vue CLI · WebSocket

## How it talks to the backend

The dev server runs on `localhost:8080` and proxies to the backend, so the browser never
makes a cross-origin request in development:

| Path | Target | Notes |
|---|---|---|
| `/ws` | `ws://localhost:8081` | WebSocket upgrade enabled for real-time chat |
| `/*` | `http://localhost:8081` | `changeOrigin: true`, prefix rewritten away |

WebSocket needs its own proxy entry — a single catch-all rule drops the upgrade handshake.

## Production build

Production builds gzip all `.js`, `.html`, and `.css` assets above 1 KB via
`compression-webpack-plugin`, keeping the originals so clients that don't request gzip
still work.

## One design note

Menu data is fetched once at login but needed on every route change. Three places could
hold it: `sessionStorage`, `localStorage`, or Vuex. Vuex is used for reads because
components need to react to changes, with the payload mirrored into browser storage so a
refresh doesn't force a re-fetch. Vuex alone loses the menu on every refresh; storage
alone isn't reactive.

## Running locally

```bash
npm install
npm run serve      # dev server on localhost:8080
npm run build      # production build with gzip assets
```
