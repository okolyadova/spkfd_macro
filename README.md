# Renins Colors — 23 September 2026

Current build: `renins-2026-09-23-51`.

## Local use

Extract all files and open `start-dashboard.cmd`. Keep `start-dashboard.ps1` next to it. Node.js 20 or newer is required; the launcher also detects the bundled Codex runtime.

The launcher verifies the server build and folder before opening the page. Port 8772 is preferred. If an older process cannot be restarted, port 8872 is used. Follow the address opened by the launcher instead of an old browser tab.

Curaleaf requires the `EODHD_API_TOKEN` server environment variable. The existing private `work/.eodhd-token` file two directories above the dashboard is also supported. A newly extracted folder may not have that private fallback. Never upload a token to GitHub or include it in an archive.

## Intraday ADTV and forecasts

The ADTV target is **RUB 51 million** for both forecast and stress-test. The covenant warning remains **RUB 50 million**.

MOEX TQBR intraday VALTODAY and VOLTODAY enter the latest 20-session average until an official closed-day history observation is available. The card has a red Preliminary label and pale-yellow values. The first history row is yellow and marked (preliminary). RENI price and capitalization continue to use the latest close.

Both scenarios include the current day's actual intraday turnover, Extra needed to reach ADTV 51 million, and seven subsequent trading days. Each intervention remains in the subsequent rolling window. Future baseline turnover uses the latest seven closed sessions; stress uses the exact minimum closed-day turnover in the displayed history month. Intraday turnover is excluded from those assumptions. Forecast dates skip weekends but do not account for future exchange holidays. Extra is calculated without rounding and displayed rounded up to whole millions.

MOEX is checked on opening, every 30 seconds while visible, and when returning to the tab. Public source data can be delayed: the card displays the MOEX update time, rather than claiming an undelayed exchange feed. An open history or forecast stays open during refresh. Other feeds refresh every 15 minutes.

All popups keep a separate close-button row, heading row, and (where applicable) action row. Main card values have consistent sizes. Macro cards are compact. The white and light-purple Renins palette is retained.

## Existing GitHub / Render deployment

Replace `index.html` and `dashboard-server.js` in the repository connected to the intended Renins Colors service. Deploy that commit. README and CHANGES are optional documentation updates. Keep the existing EODHD secret. Do not replace the existing service configuration with `render.yaml`, which describes a separate service.

Verify `/api/version`: the build must be `renins-2026-09-23-51`. Verify `/api/moex/intraday`: it must return MOEX data rather than 404. The previously recorded Render address returned 404 during this check, so the current deployment address should be checked in the user's Render account. No remote deployment was performed.

For a new service, `render.yaml` and `package.json` are included. Configure `EODHD_API_TOKEN` as a server secret.

See CHANGES.md for the changes and verification notes in Russian.
