<h1 align="center">
  <img src="Logo.png" width="120" alt="Kazemi" /><br/>
  Kazemi
</h1>

<p align="center">
  A powerful, extensible media browser for iOS & macOS — built around a JavaScript plugin system that puts you in control.
</p>

<p align="center">
  <a href="https://github.com/kazemiapp/Kazemi/releases/latest">
    <img src="https://img.shields.io/github/v/release/kazemiapp/Kazemi?label=latest&style=flat-square&color=orange" alt="Latest Release" />
  </a>
  <img src="https://img.shields.io/badge/platform-iOS%2016%2B%20%7C%20macOS%2013%2B-blue?style=flat-square" alt="Platform" />
</p>

---

## Download

### Install via AltStore (Recommended)

<a href="https://shorturl.at/kG62z" class="btn">
  <img src="https://img.shields.io/badge/Install%20with-AltStore-FF5E5B?style=for-the-badge&logo=altstore&logoColor=white" alt="Install with AltStore" />
</a>

This will open AltStore and automatically install Kazemi.

### Manual Download

| Version | Download |
|---------|----------|
| Latest stable | [⬇ Download IPA](https://github.com/kazemiapp/Kazemi/releases/latest/download/Kazemi.ipa) |
| All releases | [Releases page](https://github.com/kazemiapp/Kazemi/releases) |

### Installation

Sideload the IPA using:
- **AltStore** (recommended) - for automatic updates
- **Sideloadly** - manual installation
- Any other compatible sideloading tool

> **Note:** You'll need to refresh the app every 7 days (or 1 year with Apple Developer account) to keep it working.

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

## 💛 Support Kazemi

Your support helps cover development costs and future App Store release fees.

| Tier | Benefit |
|------|---------|
| Any amount | Your name in the **supporters list** inside the app |
| $5+ | Early access to **beta builds** before public release |
| $15+ | Priority in the **feature request board** |
| $30+ | **Lifetime Pro** — unlocks all future premium features for free |

<p align="center">
  <a href="https://ko-fi.com/kazemiapp">
    <img src="https://img.shields.io/badge/Ko--fi-Support%20Kazemi-FF5E5B?style=for-the-badge&logo=ko-fi&logoColor=white" />
  </a>
</p>

---

---

<p align="center">
  Made with ♥ — <a href="https://ko-fi.com/kazemiapp">Support the project</a>
</p>
