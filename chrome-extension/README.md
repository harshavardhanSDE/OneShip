# Chrome Extension ( Chrome API ) + React

## Folder Structure:

chrome-extension/
│
├── dist/
├── public/
│   ├── icon.png
│   └── background.jpg
│
├── src/
│   ├── background
│   │   ├── background.ts
│   │   └── events.ts
│   ├── content/
│   │   ├── content.ts
│   │   └── utils.ts
│   ├── popup/
│   │   ├── popup.html
│   │   ├── popup.css
│   │   └── popup.js
│   ├── options/
│   │   ├── options.html
│   │   ├── options.css
│   │   └── options.ts
│   ├── manifest.json
│   └── utils/
│       └── storage.ts
│
├── .gitignore
├── README.md
└── package.json


* **`src/`** : This is where all the source code lives. It contains subdirectories for background scripts, content scripts, popup UI, options page, and shared utilities.
* **`background/`** : Contains scripts that handle background tasks such as event listeners, alarms, and interactions with Chrome APIs.
* **`content/`** : Contains scripts that run in the context of web pages, interacting with DOM elements or modifying the page.
* **`popup/`** : Contains the files for the popup interface that appears when the user clicks the extension icon.
* **`options/`** : Contains the files for the options/settings page of the extension.
* **`utils/`** : Stores utility functions and shared code that can be used across different parts of the extension.
* **`public/`** : Stores static assets such as icons and images used in the extension.
* **`dist/`** : This is where the final output of your build process is placed. If you're using a bundler (e.g., Webpack or Rollup), the files get transpiled, bundled, and minified in this directory.

## Configuring ( vite.config.ts ) vite for React,

```
{
  "manifest_version": 3,
  "name": "My Chrome Extension",
  "description": "An example Chrome extension.",
  "version": "1.0.0",
  "permissions": ["storage", "tabs"],
  "action": {
    "default_popup": "src/popup/popup.html",
    "default_icon": "public/icon.png"
  },
  "background": {
    "service_worker": "src/background/background.js"
  },
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["src/content/content.js"]
    }
  ],
  "options_page": "src/options/options.html",
  "icons": {
    "16": "public/icon.png",
    "48": "public/icon.png",
    "128": "public/icon.png"
  }
}

```
