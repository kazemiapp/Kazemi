<h1 align="center">
  <img src="https://raw.githubusercontent.com/bryanzamora/kazemi-ios/main/App/Assets.xcassets/AppIcon.appiconset/icon-1024.png" width="120" alt="Kazemi" /><br/>
  Kazemi
</h1>

<p align="center">
  A powerful, extensible media browser for iOS & macOS — built around a JavaScript plugin system that puts you in control.
</p>

<p align="center">
  <a href="https://github.com/bryanzamora/kazemi-ios/releases/latest">
    <img src="https://img.shields.io/github/v/release/bryanzamora/kazemi-ios?label=latest&style=flat-square&color=orange" alt="Latest Release" />
  </a>
  <img src="https://img.shields.io/badge/platform-iOS%2016%2B%20%7C%20macOS%2013%2B-blue?style=flat-square" alt="Platform" />
  <img src="https://img.shields.io/badge/Swift-5.9-orange?style=flat-square&logo=swift" alt="Swift" />
  <img src="https://img.shields.io/badge/license-MIT-green?style=flat-square" alt="License" />
</p>

---

## Install via AltStore

> Kazemi is currently distributed via **sideloading** with AltStore. An App Store release is planned — [see below](#-help-bring-kazemi-to-the-app-store).

### Option A — AltStore Source (recommended)

1. Open **AltStore** on your device
2. Go to **Sources** → tap **+**
3. Add the following URL:
   ```
   https://bryanzamora.github.io/kazemi-ios/altstore.json
   ```
4. Find **Kazemi** in the source and tap **Install**

### Option B — Manual IPA sideload

| Version | Download |
|---------|----------|
| Latest stable | [⬇ Download IPA](https://github.com/bryanzamora/kazemi-ios/releases/latest/download/Kazemi.ipa) |
| All releases | [Releases page](https://github.com/bryanzamora/kazemi-ios/releases) |

Sideload using AltStore, Sideloadly, or any compatible tool.

---

## 💛 Help Bring Kazemi to the App Store

Publishing on the App Store requires an **Apple Developer account ($99/year)** plus significant time for compliance, review, and ongoing maintenance. Your support makes this possible.

### Why donate?

| Tier | Benefit |
|------|---------|
| Any amount | Your name in the **supporters list** inside the app |
| $5+ | Early access to **beta builds** before public release |
| $15+ | Priority in the **feature request board** |
| $30+ | **Lifetime Pro** — unlocks all future premium features for free |

> Every dollar goes directly toward the Apple Developer fee and App Store release costs.

<p align="center">
  <a href="https://ko-fi.com/bryanzamora">
    <img src="https://img.shields.io/badge/Ko--fi-Support%20Kazemi-FF5E5B?style=for-the-badge&logo=ko-fi&logoColor=white" />
  </a>
  &nbsp;
  <a href="https://www.paypal.com/donate/?hosted_button_id=XXXXXXX">
    <img src="https://img.shields.io/badge/PayPal-Donate-00457C?style=for-the-badge&logo=paypal&logoColor=white" />
  </a>
</p>

---

## What is Kazemi?

Kazemi is a **personal media library app** for iOS and macOS. It lets you browse, track, and stream content from sources you define — using a JavaScript extension system that requires no app updates to add or modify sources.

**Key features:**

- 📦 **JS Extension system** — sources and extractors are `.js` files you import yourself
- 🔍 **Browse, search & filter** — genre, type, and sort controls per source
- 📚 **Personal library** — bookmark titles and track what you've watched
- ⬇️ **Background downloads** — save content for offline viewing
- 🌐 **Multi-language UI** — English, Spanish, and Chinese
- 🎨 **Dark / Light theme** — follows system appearance

---

## How Extensions Work

Extensions are plain JavaScript files. They run inside a sandboxed JavaScriptCore context — no remote code execution, no network access beyond what the extension explicitly requests.

There are two kinds:

| Kind | File | Purpose |
|------|------|---------|
| **Source** | `mysource.js` | Browses a catalog, fetches titles and episode lists |
| **Extractor** | `myextractor.js` | Resolves an embed URL to a direct stream |

### Source extension — minimal example

```js
const SOURCE = {
  id: "mysource",
  name: "My Source",
  baseUrl: "https://example.com",
  language: "en",
  version: "1.0.0",
  iconUrl: "https://example.com/favicon.ico",
  contentKind: "series"
};

function fetchPopular(page) {
  const html = http.get(SOURCE.baseUrl + "/popular?page=" + page);
  return parseDirectory(html);
}

function fetchLatest(page) {
  const html = http.get(SOURCE.baseUrl + "/latest?page=" + page);
  return parseDirectory(html);
}

function fetchSearch(query, page, filters) {
  const url = SOURCE.baseUrl + "/search?q=" + encodeURIComponent(query) + "&page=" + page;
  const html = http.get(url);
  return parseDirectory(html);
}

function fetchAnimeDetails(id) {
  const html = http.get(SOURCE.baseUrl + "/" + id);
  // parse and return { title, synopsis, type, genres, coverUrl, ... }
}

function fetchEpisodeList(id) {
  const html = http.get(SOURCE.baseUrl + "/" + id);
  // return [{ id, number, title, pageUrl }, ...]
}

function fetchVideoList(episodeId) {
  const html = http.get(SOURCE.baseUrl + "/" + episodeId);
  // return [{ quality, embed, url }, ...]
}
```

### Extractor extension — minimal example

```js
const EXTRACTOR = {
  id: "myextractor",
  name: "MyExtractor",
  version: "1.0.0",
  domains: ["myhost.com", "myhost.to"]
};

function extractVideos(url) {
  const html = http.get(url);
  const m3u8 = html.match(/(https?:\/\/[^\s"']+\.m3u8[^\s"']*)/);
  if (!m3u8) return [];
  return [{ url: m3u8[1], quality: "HD" }];
}
```

### Available JS APIs

Inside any extension you have access to:

```js
// HTTP
http.get(url)                    // → string (HTML/text response)
http.get(url, headers)           // → string (with custom headers object)
http.post(url, body)             // → string
http.post(url, body, headers)    // → string

// Encoding
atob(base64String)               // → decoded string
btoa(string)                     // → base64 encoded string

// Logging
console.log(message)             // → appears in the app's debug log
```

---

## Installing Extensions

1. Open **Kazemi** → go to **Settings**
2. Tap **Sources** to import a source `.js` file, or **Extractors** for an extractor
3. Import from a local file or paste a direct URL to the `.js` file
4. The extension is sandboxed and persisted locally — it works offline after import

---

## Building from Source

Requirements: Xcode 15+, Swift 5.9+, macOS 13+

```bash
# Clone
git clone https://github.com/bryanzamora/kazemi-ios.git
cd kazemi-ios

# Generate Xcode project
brew install xcodegen
xcodegen generate

# Open and build
open Kazemi.xcodeproj
```

Select the `Kazemi` scheme and run on Simulator or a connected device.

---

## Project Structure

```
Kazemi/
├── App/                        # App entry point, Info.plist
├── Sources/
│   ├── KazemiCore/             # Business logic, JS runtime, models, downloads
│   │   ├── JSExtension/        # JS source + extractor runners
│   │   └── Downloads/          # HLS & MP4 download manager
│   └── KazemiUI/               # SwiftUI views and app state
├── JS/
│   ├── Fuentes/                # Bundled source extensions (.js)
│   └── Extractores/            # Bundled extractor extensions (.js)
└── Tests/
```

---

## Contributing

Pull requests are welcome. For significant changes, please open an issue first to discuss what you'd like to change.

1. Fork the repo
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Commit your changes
4. Open a pull request

---

## License

[MIT](LICENSE)

---

<p align="center">
  Made with ♥ — <a href="https://ko-fi.com/bryanzamora">Support the project</a>
</p>
