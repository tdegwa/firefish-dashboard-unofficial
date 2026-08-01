# AGENTS.md

## Cursor Cloud specific instructions

This repository is a single self-contained web app. `index.php` contains **only** HTML, CSS and vanilla JavaScript — there is **no PHP code**, no backend, no database, no package manager, and no build step (see `README.md`). All state is stored in the browser's `localStorage`.

### Services

There is exactly one thing to "run": serve `index.php` to a browser.

- Lint / test / build: **none exist** (no config, no test suite, no build tooling). Do not fabricate these commands.
- Run (development): serve the repo root over static HTTP and open `index.php` in a browser.

### Running it

Because the file has a `.php` extension, a naive static server sends it as `application/octet-stream`, which makes the browser download it instead of rendering. Serve it as `text/html` by mapping the extension. Example (port 8080):

```
python3 -c "import http.server,socketserver; h=http.server.SimpleHTTPRequestHandler; h.extensions_map['.php']='text/html'; socketserver.TCPServer(('0.0.0.0',8080),h).serve_forever()"
```

Then open `http://localhost:8080/index.php`. (Opening the file via `file://` also works, but serving is easier for automated browser testing.)

### Gotchas

- The UI is in German. Key labels: `+ Kredit hinzufügen` = add loan, `Speichern` = save, `Übersicht` = overview, `Meine Kredite` = my loans, `Einstellungen` = settings.
- A loan only counts as "active" (`Aktive Kredite`) when its maturity (start date + term) is in the future. A loan whose term has already elapsed will show as 0 active — pick a recent start date when smoke-testing.
- Chart.js and Google Fonts load from public CDNs and BTC rates come from the CoinGecko API. With no internet these are unavailable but the app still runs with bundled fallback rates; charts/live prices are simply missing.
- There are no dependencies to install, so the startup update script is effectively a no-op.
