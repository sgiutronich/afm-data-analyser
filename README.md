# AFM Data Analyser — PWA v1.0.0

Progressive Web Application for GWF acoustic flow meter data analysis, diagnostics, hardware configuration, session management and PDF reporting.

## Package Contents

```
afm-pwa/
├── index.html        ← Complete app (905 KB, all libraries bundled — works offline)
├── manifest.json     ← PWA manifest
├── sw.js             ← Service Worker (offline caching)
├── icons/            ← App icons 72px–512px
└── README.md
```

## Features

| Feature | Notes |
|---------|-------|
| 10 analysis views | Overview, Flow, Level, Velocity, Signal Quality, Diagnostics, Analysis, Parameters, Hardware Settings, Raw Data |
| Expert analysis engine | Smart dropout detection, parameter recommendations vs manual, service alerts |
| DE / EN language | Toggle in header |
| **Fully offline** | All libraries bundled — Chart.js, PapaParse, jsPDF. No CDN required after install |
| **Installable PWA** | Add to Home Screen on iOS/Android/desktop |
| **Saved sessions** | IndexedDB — save/reload analysis sessions across app restarts |
| **PDF reports** | One-click A4 branded report: health score, recommendations, path geometry, DSP table |
| Mobile-first | Responsive layout, hamburger nav, bottom sheet sessions panel |
| Offline indicator | Red bar when no internet connection |
| Update prompt | Toast when new version is deployed |

## Deployment (HTTPS required for PWA)

```bash
# Any static web server:
npx serve . -p 8080
# or
python3 -m http.server 8080

# Nginx
server {
    listen 443 ssl;
    server_name afm.yourdomain.com;
    root /var/www/afm-pwa;
    index index.html;
    location / { try_files $uri /index.html; }
    location ~* \.(png|json|js)$ { expires 1y; }
    add_header Service-Worker-Allowed "/";
}
```

**Note:** Service Worker requires HTTPS. For local testing, `localhost` is exempt.

## Supported File Formats

| File | Format | Notes |
|------|--------|-------|
| Main data | `*-data_*.csv` | Monthly export, comma-delimited, metric |
| Choose-data | `*_DT-*.csv` | Semicolon-delimited, imperial → auto-converted |
| Diagnostics | `*-diag_*.csv` | Signal diagnostics |
| Parameters | `*_params.txt` | JSON device configuration |

## Cloud Integration Points

For your developers when connecting to the cloud system:

```javascript
// Replace local file loading with API calls:
loadFile(input)        → fetch CSV from cloud API
loadParamsFile(input)  → fetch device config from registry
saveSession()          → POST to cloud sessions API
exportCSV()            → PUT to cloud storage

// State object to populate from API:
state.raw              // Array of measurement rows
state.params           // Device parameter object
state.filename         // String
state.detectedPaths    // Array of path numbers
```

## Version Updates

Increment `CACHE = 'afm-v1.0.0'` in `sw.js` on each deployment.
Users see an "Update available" toast and can reload without clearing cache.

## Browser Support

| Browser | Install | Offline | Sessions | PDF |
|---------|---------|---------|----------|-----|
| Chrome 67+ | ✓ | ✓ | ✓ | ✓ |
| Edge 79+ | ✓ | ✓ | ✓ | ✓ |
| Safari iOS 14.3+ | ✓ | ✓ | ✓ | ✓ |
| Firefox | — | ✓ | ✓ | ✓ |
| Samsung Internet | ✓ | ✓ | ✓ | ✓ |
