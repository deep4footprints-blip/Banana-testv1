# Banana NSE — mobile stock scanner starter

Mobile-first Android app scaffold inspired by the general workflow of BananaPatterns. It is an independent implementation and does not copy proprietary source or assets.

## Current capabilities
- Responsive dark mobile UI with Overview, Scanner, Watchlist and Charts tabs.
- Local watchlist persistence.
- API base URL configuration inside the app.
- Quote fetching via `GET /stock/list?symbols=RELIANCE.NS,TCS.NS&res=num`.
- Chart request UI for `GET /chart?symbol=RELIANCE.NS&timeframe=1Y` when supported by the configured API.
- GitHub Actions workflow builds a debug APK and uploads it as an Actions artifact.

## Important data-source limitation
The linked `0xramm/Indian-Stock-Market-API` README documents quote/search/symbol endpoints. A GitHub repository is not automatically a hosted API service, so you must deploy its code or use a legitimate running endpoint and enter its base URL in app Settings. The original repo does not document `/chart`; historical candle charting and base/VCP detection are not functional until an OHLCV endpoint is configured. This starter deliberately does not invent pattern signals.

Before public deployment, verify the upstream API's current response schema, data freshness, rate limits, CORS behaviour, license and upstream Yahoo Finance/exchange data terms. Do not treat the feed as guaranteed real-time or exchange-authorized.

## Publish from a phone
1. Download and extract this project ZIP.
2. Create a new GitHub repository, e.g. `banana-nse-app`.
3. Upload the extracted files and folders to the repository root; commit to branch `main`.
4. Open the repo's **Actions** tab and enable Actions if prompted.
5. Select **Build Android APK** → **Run workflow**. A push to `main` that changes app files also triggers a build.
6. Open the completed workflow run → **Artifacts** → download `banana-nse-debug-apk`, extract it and install `app-debug.apk` on Android. Android may ask you to allow installation from that source.

## Local development
Requires Node.js 20+, Java 21 and Android SDK for APK builds.

```bash
npm install
npm run build
npx cap add android
npx cap sync android
```

## Data endpoint expectations
The quote client expects `/stock/list?symbols=...&res=num` and tolerates common response shapes (`array`, `data`, `results`, `stocks`). Inspect the actual response and adjust `src/main.js` if needed. For charting, it expects a response containing `candles` or `data`, with `timestamp`, `open`, `high`, `low`, `close`, and `volume` fields.

## Roadmap
1. Verify/deploy data API and map response schema.
2. Add durable historical OHLCV storage and corporate-action handling.
3. Implement explicit, testable VCP/base/pivot/breakout rules.
4. Add nightly NSE universe refresh, breadth, relative strength and survivorship-bias-aware backtests.
5. Add alerting, error monitoring and optional account sync.

For personal research only; not investment advice or a broker/order-placement app.
