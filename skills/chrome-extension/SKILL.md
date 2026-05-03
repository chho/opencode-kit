---
name: chrome-extension
description: Chrome Extension development guide for Manifest V3 - manifest.json configuration, service workers, content scripts, permissions, messaging, storage API, UI (popup, side panel, action, context menus), declarative net request, tabs/windows management, debugging, and publishing. Load this when building or debugging a Chrome extension.
---

# Chrome Extension Development Guide (Manifest V3)

> This skill contains essential Chrome Extension development documentation for building Manifest V3 extensions.

<SYSTEM>This is the full developer documentation for Chrome Extensions (Manifest V3)</SYSTEM>

# Overview

Chrome extensions are built with standard web technologies (HTML, CSS, JavaScript) and have access to Chrome Extension APIs beyond what regular web pages can do. Extensions can customize the UI, observe browser events, modify web content, and interact with the browser's features.

**Manifest V3 (MV3)** is the current and only supported manifest version. All new extensions must use `manifest_version: 3`.

Beginning in Chrome 146, all Chrome Extension APIs are also available under the `browser` namespace (e.g., `browser.tabs.create({})`) as an alias for the `chrome` namespace.

---

# Project Structure

A typical Chrome extension project:

```
my-extension/
├── manifest.json          # Required - extension metadata and configuration
├── service-worker.js      # Background event handler
├── content-script.js      # Runs in context of web pages
├── popup.html             # Toolbar popup UI
├── popup.js               # Popup logic
├── options.html           # Extension options page
├── options.js             # Options page logic
├── images/
│   ├── icon-16.png
│   ├── icon-48.png
│   └── icon-128.png
└── styles/
    └── content.css
```

---

# manifest.json Reference

Every Chrome extension **must** have a `manifest.json` in its root directory.

## Required Fields

```json
{
  "manifest_version": 3,
  "name": "My Extension",
  "version": "1.0.0"
}
```

- `manifest_version`: Must be `3` (only supported value).
- `name`: Extension name. Max 75 characters. Supports localization.
- `version`: Dot-separated integers (e.g., `"1.0.0"`).

## Chrome Web Store Required Fields

```json
{
  "description": "A description of my extension.",
  "icons": {
    "16": "images/icon-16.png",
    "48": "images/icon-48.png",
    "128": "images/icon-128.png"
  }
}
```

- `description`: Max 132 characters.
- `icons`: Maps size (as string) to file paths.

## Key Optional Fields

### Service Worker (Background Script)

```json
{
  "background": {
    "service_worker": "service-worker.js"
  }
}
```

### Action (Toolbar Icon + Popup)

```json
{
  "action": {
    "default_icon": {
      "16": "images/icon-16.png",
      "48": "images/icon-48.png"
    },
    "default_title": "Click me",
    "default_popup": "popup.html"
  }
}
```

### Content Scripts (Static Declaration)

```json
{
  "content_scripts": [
    {
      "matches": ["https://*.example.com/*"],
      "css": ["styles/content.css"],
      "js": ["content-script.js"],
      "run_at": "document_idle"
    }
  ]
}
```

### Permissions

```json
{
  "permissions": [
    "storage",
    "activeTab",
    "scripting"
  ],
  "optional_permissions": [
    "topSites"
  ],
  "host_permissions": [
    "https://*.example.com/*"
  ],
  "optional_host_permissions": [
    "https://*/*"
  ]
}
```

### Side Panel

```json
{
  "side_panel": {
    "default_path": "sidepanel.html"
  }
}
```

### Commands (Keyboard Shortcuts)

```json
{
  "commands": {
    "_execute_action": {
      "suggested_key": {
        "default": "Ctrl+Shift+Y"
      }
    }
  }
}
```

### Override Chrome Pages

```json
{
  "chrome_url_overrides": {
    "newtab": "newtab.html"
  }
}
```

### Web Accessible Resources

```json
{
  "web_accessible_resources": [
    {
      "resources": ["images/*.png", "styles/*.css"],
      "matches": ["https://*.example.com/*"]
    }
  ]
}
```

### Options Page

```json
{
  "options_ui": {
    "page": "options.html",
    "open_in_tab": true
  }
}
```

### Declarative Net Request

```json
{
  "declarative_net_request": {
    "rule_resources": [
      {
        "id": "ruleset_1",
        "enabled": true,
        "path": "rules.json"
      }
    ]
  }
}
```

### Other Fields

| Field | Description |
|---|---|
| `"content_security_policy"` | Restricts scripts, styles, and resources the extension can use |
| `"cross_origin_embedder_policy"` | Cross-Origin-Embedder-Policy for extension pages |
| `"cross_origin_opener_policy"` | Cross-Origin-Opener-Policy for extension pages |
| `"default_locale"` | Default language for multi-locale extensions (e.g., `"en"`) |
| `"devtools_page"` | HTML file for DevTools extension |
| `"externally_connectable"` | Which pages/extensions can message yours |
| `"homepage_url"` | Extension homepage URL |
| `"incognito"` | Incognito behavior: `"spanning"`, `"split"`, `"not_allowed"` |
| `"key"` | Extension ID for development |
| `"minimum_chrome_version"` | Minimum Chrome version required |
| `"oauth2"` | OAuth 2.0 config with `client_id` and `scopes` |
| `"omnibox"` | Register address bar keyword |
| `"sandbox"` | Sandbox pages without extension API access |
| `"short_name"` | Shortened name, max 12 characters |
| `"tts_engine"` | Register as text-to-speech engine |
| `"update_url"` | URL for extension updates (self-hosted) |
| `"version_name"` | Human-readable version (e.g., `"1.0 beta"`) |

---

# Service Workers

The extension service worker (`service-worker.js`) is the extension's **central event handler**, declared in `manifest.json` under `"background"`.

## Key Characteristics

- **Event-based**: Loaded on demand, unloaded when idle.
- **No DOM access**: Use the Offscreen API (`chrome.offscreen`) for DOM operations.
- **Ephemeral**: Can be terminated at any time. Do not rely on global state persisting.
- **No remotely hosted code**: All logic must be bundled in the extension.

## Event Listener Pattern

**Always register event listeners at the top level** (not inside callbacks or async functions), because the service worker may be restarted and only top-level listeners are guaranteed to be re-registered:

```javascript
// service-worker.js

// CORRECT: Top-level listener registration
chrome.action.onClicked.addListener((tab) => {
  chrome.scripting.executeScript({
    target: { tabId: tab.id },
    files: ["content-script.js"]
  });
});

chrome.runtime.onMessage.addListener((message, sender, sendResponse) => {
  if (message.type === "GET_DATA") {
    fetchData().then(sendResponse);
    return true; // Keep channel open for async response
  }
});

// WRONG: Listener inside async function (may not re-register on restart)
// async function init() {
//   chrome.action.onClicked.addListener(...); // NOT guaranteed
// }
```

## State Persistence

Use `chrome.storage` API to persist state across service worker restarts:

```javascript
// service-worker.js
const storageCache = {};

const initStorageCache = chrome.storage.local.get().then((items) => {
  Object.assign(storageCache, items);
});

chrome.action.onClicked.addListener(async (tab) => {
  await initStorageCache;
  storageCache.clickCount = (storageCache.clickCount || 0) + 1;
  await chrome.storage.local.set(storageCache);
});
```

## Debugging

1. Go to `chrome://extensions`
2. Enable "Developer mode"
3. Click "Inspect views: service worker" on your extension
4. Use Console, Network, Sources panels in DevTools

---

# Content Scripts

Content scripts run in the context of web pages. They can read and modify the DOM and pass information to the parent extension.

## Isolated Worlds

Content scripts run in an **isolated world** — they share the DOM with the page but have separate JavaScript environments. Variables in content scripts are not visible to the page and vice versa.

## Accessible Extension APIs

Content scripts can directly access only these APIs:
- `chrome.dom`
- `chrome.i18n`
- `chrome.storage`
- `chrome.runtime.connect()`, `chrome.runtime.sendMessage()`
- `chrome.runtime.getManifest()`, `chrome.runtime.getURL()`
- `chrome.runtime.id`
- `chrome.runtime.onConnect`, `chrome.runtime.onMessage`

For other APIs, communicate with the service worker via messaging.

## Three Ways to Inject Content Scripts

### 1. Static Declaration (manifest.json)

For scripts that should auto-run on known pages:

```json
{
  "content_scripts": [
    {
      "matches": ["https://*.example.com/*"],
      "css": ["styles.css"],
      "js": ["content-script.js"],
      "run_at": "document_idle",
      "all_frames": false,
      "exclude_matches": ["*://*/*admin*"],
      "include_globs": ["*example.com/???s/*"],
      "exclude_globs": ["*science*"]
    }
  ]
}
```

### 2. Dynamic Registration (chrome.scripting API)

For scripts with dynamic match patterns:

```javascript
// service-worker.js
await chrome.scripting.registerContentScripts([{
  id: "my-script",
  js: ["content.js"],
  persistAcrossSessions: false,
  matches: ["*://example.com/*"],
  runAt: "document_start"
}]);

// Update
await chrome.scripting.updateContentScripts([{
  id: "my-script",
  excludeMatches: ["*://admin.example.com/*"]
}]);

// Get all registered
const scripts = await chrome.scripting.getRegisteredContentScripts();

// Unregister
await chrome.scripting.unregisterContentScripts({ ids: ["my-script"] });
```

Requires `"scripting"` permission.

### 3. Programmatic Injection

For on-demand injection in response to events:

```json
{
  "permissions": ["activeTab", "scripting"],
  "background": {
    "service_worker": "service-worker.js"
  },
  "action": {
    "default_title": "Inject Script"
  }
}
```

```javascript
// service-worker.js

// Inject a file
chrome.action.onClicked.addListener((tab) => {
  chrome.scripting.executeScript({
    target: { tabId: tab.id },
    files: ["content-script.js"]
  });
});

// Inject a function
function injectedFunction(color) {
  document.body.style.backgroundColor = color;
}

chrome.action.onClicked.addListener((tab) => {
  chrome.scripting.executeScript({
    target: { tabId: tab.id },
    func: injectedFunction,
    args: ["orange"]
  });
});
```

**Important**: Injected functions are **copies** — they must be self-contained. No closures over external variables.

## run_at Values

| Value | Description |
|---|---|
| `"document_idle"` | **Default/Preferred.** Between `document_end` and after `window.onload`. DOM is complete. |
| `"document_start"` | After CSS injection, before any DOM construction or other scripts. |
| `"document_end"` | After DOM is complete, before subresources load. |

## Accessing Extension Assets from Content Scripts

```javascript
// JavaScript
const imageUrl = chrome.runtime.getURL("images/my_image.png");
```

```css
/* CSS */
body {
  background-image: url('chrome-extension://__MSG_@@extension_id__/background.png');
}
```

Assets must be declared as `"web_accessible_resources"` in manifest.

---

# Permissions

Permissions limit the extension's capabilities and protect users. Declare them in `manifest.json`.

## Four Permission Categories

| Manifest Key | Granted | Purpose |
|---|---|---|
| `"permissions"` | At install time | API access (e.g., `"storage"`, `"tabs"`) |
| `"optional_permissions"` | At runtime | APIs requested only when needed |
| `"host_permissions"` | At install time | URL match patterns for host access |
| `"optional_host_permissions"` | At runtime | Host access requested when needed |

## Common API Permissions

| Permission | API / Description |
|---|---|
| `"activeTab"` | Temporary access to current tab (no host permission warning) |
| `"storage"` | `chrome.storage` API |
| `"tabs"` | `chrome.tabs` API (host permissions needed for `url`, `title`, `favIconUrl`) |
| `"scripting"` | `chrome.scripting` for programmatic injection |
| `"contextMenus"` | `chrome.contextMenus` for context menu items |
| `"bookmarks"` | `chrome.bookmarks` for bookmark management |
| `"history"` | `chrome.history` for browsing history |
| `"cookies"` | `chrome.cookies` (also needs host permissions) |
| `"alarms"` | `chrome.alarms` for scheduled code execution |
| `"notifications"` | `chrome.notifications` for system notifications |
| `"sidePanel"` | `chrome.sidePanel` for browser side panel |
| `"commands"` | `chrome.commands` for keyboard shortcuts |
| `"identity"` | `chrome.identity` for OAuth 2.0 authentication |
| `"omnibox"` | `chrome.omnibox` for address bar keyword |
| `"management"` | `chrome.management` for managing extensions |
| `"downloads"` | `chrome.downloads` for download management |
| `"declarativeNetRequest"` | `chrome.declarativeNetRequest` for network request rules |
| `"webRequest"` | `chrome.webRequest` for network request monitoring |
| `"offscreen"` | `chrome.offscreen` for hidden DOM documents |
| `"debugger"` | `chrome.debugger` for remote debugging protocol |
| `"desktopCapture"` | Screen/window/tab capture |
| `"system.storage"` | `chrome.system.storage` for storage device info |
| `"unlimitedStorage"` | Bypass `storage.local` 10 MB quota |

## Host Permissions Enable

- Cross-origin `fetch()` from service worker and extension pages
- Reading `url`, `title`, `favIconUrl` via `chrome.tabs`
- Programmatic content script injection
- Network request monitoring with `chrome.webRequest`
- Cookie access with `chrome.cookies`
- Request modification with `chrome.declarativeNetRequest`

## Requesting Optional Permissions at Runtime

```javascript
// Check if permission is granted
const hasPermission = await chrome.permissions.contains({
  permissions: ["topSites"]
});

// Request permission
const granted = await chrome.permissions.request({
  permissions: ["topSites"]
});

// Remove permission
await chrome.permissions.remove({
  permissions: ["topSites"]
});
```

## Special Access

- **File URLs** (`file://`): Users must manually enable on extension details page.
- **Incognito mode**: Users must manually enable on extension details page.

---

# Messaging

Communication between extension components uses two patterns: one-time requests and long-lived connections.

## One-Time Requests

### Content Script → Service Worker

```javascript
// content-script.js (sender)
const response = await chrome.runtime.sendMessage({ type: "GET_DATA" });
console.log(response);
```

```javascript
// service-worker.js (receiver)
chrome.runtime.onMessage.addListener((message, sender, sendResponse) => {
  if (message.type === "GET_DATA") {
    fetchData().then(data => sendResponse({ data }));
    return true; // Required for async sendResponse
  }
});
```

### Service Worker → Content Script

```javascript
// service-worker.js (sender)
const response = await chrome.tabs.sendMessage(tabId, { type: "ACTION" });
```

```javascript
// content-script.js (receiver)
chrome.runtime.onMessage.addListener((message, sender, sendResponse) => {
  if (message.type === "ACTION") {
    sendResponse({ result: "done" });
  }
});
```

### Async Response Options

**Option A: Return `true`** (all Chrome versions)

```javascript
chrome.runtime.onMessage.addListener((message, sender, sendResponse) => {
  fetchData().then(sendResponse);
  return true; // Keeps channel open
});
```

**Option B: Return a Promise** (Chrome 146+)

```javascript
chrome.runtime.onMessage.addListener(async (message, sender) => {
  const data = await fetchData();
  return { data }; // Resolved promise value sent as response
});
```

## Long-Lived Connections (Ports)

For multiple messages over a persistent channel:

### Content Script → Service Worker

```javascript
// content-script.js
const port = chrome.runtime.connect({ name: "knockknock" });
port.postMessage({ joke: "Knock knock" });
port.onMessage.addListener((msg) => {
  if (msg.question === "Who's there?") {
    port.postMessage({ answer: "Madame" });
  }
});
```

```javascript
// service-worker.js
chrome.runtime.onConnect.addListener((port) => {
  if (port.name !== "knockknock") return;
  port.onMessage.addListener((msg) => {
    if (msg.joke === "Knock knock") {
      port.postMessage({ question: "Who's there?" });
    }
  });
});
```

### Service Worker → Content Script

```javascript
// service-worker.js
const port = chrome.tabs.connect(tabId, { name: "my-channel" });
port.postMessage({ command: "START" });
```

## Cross-Extension Messaging

```javascript
// Sender (must know target extension ID)
const targetId = "abcdefghijklmnoabcdefhijklmnoabc";
const response = await chrome.runtime.sendMessage(targetId, { getTargetData: true });
```

```javascript
// Receiver
chrome.runtime.onMessageExternal.addListener((request, sender, sendResponse) => {
  if (sender.id !== allowedExtensionId) return;
  sendResponse({ data: targetData });
});
```

## Web Page → Extension Messaging

```json
// manifest.json
{
  "externally_connectable": {
    "matches": ["https://*.example.com/*"]
  }
}
```

```javascript
// Web page
chrome.runtime.sendMessage(extensionId, { action: "open" }, (response) => { });
```

```javascript
// service-worker.js
chrome.runtime.onMessageExternal.addListener((request, sender, sendResponse) => {
  sendResponse({ success: true });
});
```

**Note**: Extensions cannot send messages TO web pages.

## Key Messaging Rules

- Messages must be **JSON-serializable** (uses `JSON.stringify`).
- Maximum message size: **64 MiB**.
- `undefined` is serialized as `null`.
- Multiple listeners: only the **first** to respond wins.
- Content script messages are **less trustworthy** — always validate and sanitize.

## Messaging API Quick Reference

| API | Direction |
|---|---|
| `chrome.runtime.sendMessage()` | Content script → Extension |
| `chrome.tabs.sendMessage(tabId)` | Extension → Content script in tab |
| `chrome.runtime.onMessage` | Listen for one-time messages |
| `chrome.runtime.connect()` | Open channel (content script → extension) |
| `chrome.tabs.connect(tabId)` | Open channel (extension → content script) |
| `chrome.runtime.onConnect` | Listen for incoming connections |
| `chrome.runtime.onMessageExternal` | Messages from other extensions/web pages |
| `chrome.runtime.onConnectExternal` | Connections from other extensions/web pages |
| `chrome.runtime.connectNative()` | Long-lived channel to native app |
| `chrome.runtime.sendNativeMessage()` | One-time message to native app |
| `port.postMessage()` | Send through a port |
| `port.onMessage` | Listen on a port |
| `port.onDisconnect` | Port disconnection event |
| `port.disconnect()` | Close a port |

---

# Storage API

Requires `"storage"` permission.

## Four Storage Areas

| Area | Persistence | Quota | Syncs? | Content Script Access |
|---|---|---|---|---|
| `storage.local` | Persisted, cleared on uninstall | **10 MB** | No | Yes (default) |
| `storage.sync` | Persisted, synced across Chrome | **100 KB total**, 8 KB/item, 512 items | Yes | Yes (default) |
| `storage.session` | In memory, cleared on restart | **10 MB** | No | No (default) |
| `storage.managed` | Read-only, set by enterprise policy | N/A | Via policy | Yes (default) |

Use `"unlimitedStorage"` permission to bypass `storage.local` quota.

## API Methods

```javascript
// Write
await chrome.storage.local.set({ key: "value", count: 42 });

// Read
const result = await chrome.storage.local.get(["key"]);
const allData = await chrome.storage.local.get();

// Remove specific keys
await chrome.storage.local.remove(["key"]);

// Clear all
await chrome.storage.local.clear();

// Get bytes in use
const bytes = await chrome.storage.local.getBytesInUse(["key"]);

// Get all keys
const keys = await chrome.storage.local.getKeys();
```

## Listen for Changes

```javascript
chrome.storage.onChanged.addListener((changes, areaName) => {
  for (const [key, { oldValue, newValue }] of Object.entries(changes)) {
    console.log(`${areaName}.${key}: ${oldValue} → ${newValue}`);
  }
});
```

## Session Storage Access Level

By default, `storage.session` is not accessible from content scripts. To enable:

```javascript
await chrome.storage.session.setAccessLevel({
  accessLevel: "TRUSTED_AND_UNTRUSTED_CONTEXTS"
});
```

## Storage Best Practices

- Use `storage.local` for large data.
- Use `storage.sync` for user settings that should sync.
- Use `storage.session` for temporary data (e.g., service worker state).
- Do NOT use `window.localStorage` — it doesn't work in service workers and is shared with host pages in content scripts.

---

# UI Components

## Popup (Action API)

```json
{
  "action": {
    "default_popup": "popup.html",
    "default_icon": {
      "16": "icon-16.png",
      "48": "icon-48.png"
    },
    "default_title": "My Extension"
  }
}
```

```javascript
// service-worker.js — listen for action click (no popup defined)
chrome.action.onClicked.addListener((tab) => {
  // Action when popup is NOT defined
});

// Dynamically set badge text and color
chrome.action.setBadgeText({ text: "ON" });
chrome.action.setBadgeBackgroundColor({ color: "#00FF00" });
```

## Side Panel

```json
{
  "permissions": ["sidePanel"],
  "side_panel": {
    "default_path": "sidepanel.html"
  }
}
```

```javascript
// service-worker.js
chrome.sidePanel.setPanelBehavior({ openPanelOnActionClick: true });

// Open programmatically
chrome.sidePanel.open({ tabId: tab.id });
```

## Context Menus

```json
{
  "permissions": ["contextMenus"]
}
```

```javascript
// service-worker.js
chrome.runtime.onInstalled.addListener(() => {
  chrome.contextMenus.create({
    id: "sample",
    title: "My Menu Item",
    contexts: ["selection"]
  });
});

chrome.contextMenus.onClicked.addListener((info, tab) => {
  if (info.menuItemId === "sample") {
    console.log("Selected text:", info.selectionText);
  }
});
```

## Notifications

```json
{
  "permissions": ["notifications"]
}
```

```javascript
chrome.notifications.create("notify-id", {
  type: "basic",
  iconUrl: "icon-128.png",
  title: "Notification Title",
  message: "Notification body text"
});
```

## Omnibox

```json
{
  "omnibox": { "keyword": "myext" }
}
```

```javascript
chrome.omnibox.onInputChanged.addListener((text, suggest) => {
  suggest([
    { content: text + " one", description: "First suggestion" },
    { content: text + " two", description: "Second suggestion" }
  ]);
});

chrome.omnibox.onInputEntered.addListener((text) => {
  chrome.tabs.create({ url: `https://example.com/search?q=${text}` });
});
```

---

# Tabs & Windows

## Tabs API

```json
{
  "permissions": ["tabs"]
}
```

```javascript
// Create tab
const tab = await chrome.tabs.create({ url: "https://example.com" });

// Query tabs
const tabs = await chrome.tabs.query({ active: true, currentWindow: true });

// Update tab
await chrome.tabs.update(tabId, { url: "https://example.com" });

// Remove tab
await chrome.tabs.remove(tabId);

// Listen for tab changes
chrome.tabs.onCreated.addListener((tab) => { });
chrome.tabs.onUpdated.addListener((tabId, changeInfo, tab) => { });
chrome.tabs.onRemoved.addListener((tabId, removeInfo) => { });
```

**Note**: Reading `tab.url`, `tab.title`, `tab.favIconUrl` requires **host permissions** for that URL.

## Tab Groups

```javascript
const groupId = await chrome.tabs.group({ tabIds: [tabId1, tabId2] });
await chrome.tabGroups.update(groupId, { title: "My Group", color: "blue" });
```

## Windows API

```javascript
const win = await chrome.windows.create({ url: "https://example.com", type: "popup" });
const currentWin = await chrome.windows.getCurrent();
const allWindows = await chrome.windows.getAll();
```

---

# Declarative Net Request

Block or modify network requests using declarative rules (no need to intercept requests):

```json
{
  "permissions": ["declarativeNetRequest"],
  "declarative_net_request": {
    "rule_resources": [{
      "id": "ruleset_1",
      "enabled": true,
      "path": "rules.json"
    }]
  },
  "host_permissions": ["https://*/*"]
}
```

```json
// rules.json
[
  {
    "id": 1,
    "priority": 1,
    "action": { "type": "block" },
    "condition": {
      "urlFilter": "||ads.example.com",
      "resourceTypes": ["main_frame", "script"]
    }
  },
  {
    "id": 2,
    "priority": 2,
    "action": {
      "type": "redirect",
      "redirect": { "url": "https://example.com/blocked.html" }
    },
    "condition": {
      "urlFilter": "||tracker.com",
      "resourceTypes": ["script"]
    }
  }
]
```

---

# Alarms API

Schedule code to run periodically (useful for service workers since `setTimeout` doesn't persist):

```json
{
  "permissions": ["alarms"]
}
```

```javascript
// Create alarm
chrome.alarms.create("my-alarm", { periodInMinutes: 30 });

// Listen
chrome.alarms.onAlarm.addListener((alarm) => {
  if (alarm.name === "my-alarm") {
    console.log("Alarm fired");
  }
});

// Clear
chrome.alarms.clear("my-alarm");
```

---

# i18n (Internationalization)

```json
{
  "default_locale": "en"
}
```

```
_locales/en/messages.json:
{
  "extName": {
    "message": "My Extension"
  },
  "greeting": {
    "message": "Hello, $user$!",
    "placeholders": {
      "user": { "content": "$1" }
    }
  }
}
```

```json
// manifest.json uses __MSG_ prefix
{
  "name": "__MSG_extName__"
}
```

```javascript
// JavaScript
const msg = chrome.i18n.getMessage("greeting", "World");
```

---

# Offscreen API

Service workers have no DOM access. Use the Offscreen API for DOM operations:

```json
{
  "permissions": ["offscreen"]
}
```

```javascript
// service-worker.js
await chrome.offscreen.createDocument({
  url: "offscreen.html",
  reasons: ["AUDIO_PLAYBACK"],
  justification: "Playing notification sound"
});

// Send message to offscreen document
chrome.runtime.sendMessage({ type: "PLAY_AUDIO", url: "notification.mp3" });
```

```javascript
// offscreen.js
chrome.runtime.onMessage.addListener((msg) => {
  if (msg.type === "PLAY_AUDIO") {
    const audio = new Audio(msg.url);
    audio.play();
  }
});
```

---

# Common Patterns

## Complete MV3 Extension Template

```json
// manifest.json
{
  "manifest_version": 3,
  "name": "My Extension",
  "version": "1.0.0",
  "description": "A Chrome extension example",
  "icons": {
    "16": "images/icon-16.png",
    "48": "images/icon-48.png",
    "128": "images/icon-128.png"
  },
  "permissions": [
    "storage",
    "activeTab",
    "scripting"
  ],
  "background": {
    "service_worker": "service-worker.js"
  },
  "action": {
    "default_popup": "popup.html",
    "default_icon": {
      "16": "images/icon-16.png",
      "48": "images/icon-48.png"
    }
  }
}
```

## Full Chrome Extension API Reference

| API | Description |
|---|---|
| `chrome.action` | Toolbar icon and popup control |
| `chrome.alarms` | Scheduled code execution |
| `chrome.bookmarks` | Bookmark management |
| `chrome.browsingData` | Clear browsing data |
| `chrome.commands` | Keyboard shortcuts |
| `chrome.contentSettings` | Per-site content settings |
| `chrome.contextMenus` | Context menu items |
| `chrome.cookies` | Cookie management |
| `chrome.debugger` | Remote debugging protocol |
| `chrome.declarativeNetRequest` | Declarative network request rules |
| `chrome.desktopCapture` | Screen/window capture |
| `chrome.devtools.*` | DevTools extension APIs |
| `chrome.dns` | DNS resolution |
| `chrome.dom` | Special DOM APIs for extensions |
| `chrome.downloads` | Download management |
| `chrome.events` | Common event types |
| `chrome.extensionTypes` | Type declarations |
| `chrome.history` | Browsing history |
| `chrome.i18n` | Internationalization |
| `chrome.identity` | OAuth 2.0 authentication |
| `chrome.idle` | Idle state detection |
| `chrome.management` | Manage installed extensions |
| `chrome.notifications` | System notifications |
| `chrome.offscreen` | Offscreen documents for DOM access |
| `chrome.omnibox` | Address bar keyword |
| `chrome.permissions` | Runtime permission management |
| `chrome.privacy` | Privacy settings control |
| `chrome.proxy` | Proxy settings |
| `chrome.readingList` | Reading list management |
| `chrome.runtime` | Lifecycle, messaging, manifest info |
| `chrome.scripting` | Content script injection |
| `chrome.search` | Search functionality |
| `chrome.sessions` | Tab/session management |
| `chrome.sidePanel` | Browser side panel |
| `chrome.storage` | Extension data storage |
| `chrome.system.cpu` | CPU information |
| `chrome.system.display` | Display information |
| `chrome.system.memory` | Memory information |
| `chrome.system.storage` | Storage device information |
| `chrome.tabCapture` | Tab audio/video capture |
| `chrome.tabGroups` | Tab group management |
| `chrome.tabs` | Tab management |
| `chrome.topSites` | Most visited sites |
| `chrome.tts` | Text-to-speech |
| `chrome.userScripts` | User script management |
| `chrome.webNavigation` | Navigation events |
| `chrome.webRequest` | Network request monitoring |
| `chrome.windows` | Window management |

---

# Best Practices & Security

1. **Single purpose**: Extensions should fulfill one narrowly defined purpose.
2. **Least privilege**: Only request permissions you actually need. Use optional permissions where possible.
3. **No remotely hosted code**: All JavaScript must be bundled in the extension (MV3 requirement).
4. **Validate content script messages**: Treat them as potentially malicious. Sanitize all input.
5. **Avoid `eval()`**: Use `JSON.parse()` instead. Blocked by CSP in MV3.
6. **Avoid string-form `setTimeout`**: Use closure form instead.
7. **Use `chrome.storage`** instead of `window.localStorage`.
8. **Register event listeners at top level** in service workers.
9. **Use HTTPS** for all external communication.
10. **Test in incognito mode** if your extension might be used there.
