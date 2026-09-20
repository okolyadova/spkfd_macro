# Financial Market Dashboard — studio edition dated 18 September 2026

This is a separate version of the dashboard. The existing published site is unchanged. The report date shown on the page updates each day when data reloads.

## Open locally

On Windows, double-click `start-dashboard.cmd`. It starts the local data service and opens <http://127.0.0.1:8770/>. Keep the computer running while viewing this local address.

The EODHD token is read from the existing private `work/.eodhd-token` file two folders above this directory, or from the `EODHD_API_TOKEN` environment variable. Never add a token to the HTML or a GitHub repository.

## Update the existing Render link

To replace the dashboard at `https://new-macro-dashboard.onrender.com/`, upload this directory's `index.html` and `dashboard-server.js` to the **root of the GitHub repository already connected to that Render service**, replacing files with the same names. Commit the changes to the branch connected to Render (usually `main`). Render normally deploys that commit automatically; if auto deploy is disabled, open the existing service's **Deploys** page and choose **Manual Deploy → Deploy latest commit**. The EODHD secret already configured in that service remains there. Do not upload this directory's `render.yaml` for an update of the existing service: it names a different service.

## Publish separately on Render

To publish this version, put this directory's files in a **new** GitHub repository and create a new Render service from its `render.yaml`. Set the secret `EODHD_API_TOKEN` in Render. This version has a distinct Render service name (`new-macro-dashboard-v2`) so it will not replace the existing site.

The Blueprint selects Render's Free web service plan. Render puts idle Free services to sleep and displays its own startup page to the next visitor. The dashboard's loading placeholders appear only after the server has started. A paid web service plan removes idle spin-down; alternatively, a separately hosted static frontend could present a branded waiting screen while this backend wakes up.

## Data notes

The dashboard uses MOEX data for RENI, Bank of Russia for USD/RUB, EUR/RUB and the key rate, Bank of Canada for CAD/USD, and EODHD for CURA.TO. Federal Reserve target range is loaded from FRED and checked against the most recent official FOMC statement, which can publish before the FRED daily observation. Curaleaf shares use reported data from EODHD when available, with a dated SEC filing fallback. Missing data shows an error instead of an invented value.

Curaleaf market-cap YoY appears only when both comparison dates have an EODHD TSX close, same-day Bank of Canada CAD/USD, and a previously published share count. The September 2025 fallback is the SEC filing published 7 August 2025: 670,458,386 subordinate plus 93,970,705 multiple voting shares as of 4 August. The 2026 fallback is a post-split count. The calculation uses the unadjusted close and the share count from each period; the June 2026 1-for-3 reverse split does not itself change market capitalization. If a comparable base date is unavailable, the YoY item is omitted.

Each KPI card has a History control showing the previous 10 calendar days, except RENI 20D ADTV, which covers one calendar month through the latest trading date. Price, FX, policy rate and market capitalization histories use compact charts with one decimal place. The RENI 20D ADTV history recalculates the cash average for each session using that date's latest 20 trading sessions, with weekends and days without trades excluded. Its 7d Forecast fixes turnover at the latest 7-session average. Its Stress-test fixes turnover at the minimum daily turnover observed during the ADTV history month. Future exchange holidays are not known to either scenario, so forecast dates are provisional.

Both ADTV scenarios first report the unadjusted 20-session average based on their fixed turnover assumption. They then report the additional turnover needed on each date to bring the adjusted rolling 20-session average to ₽60 million, followed by adjusted total turnover and ADTV. Forecast table values use whole millions and required extra turnover is rounded upward. Each calculated intervention remains in the adjusted 20-session window on later dates.

The ADTV history table keeps a light background and shows a blue bar next to each numeric value. In the scenario tables, ADTV values below ₽60 million use dark red text. History, forecast and stress-test windows stay open until the user presses their × button. When the standard seven-day forecast falls below ₽60 million, a centered covenant-risk alert shows the required-turnover range from the standard scenario to the stress scenario. Its “See 7 days forecast” button opens the standard forecast table.
