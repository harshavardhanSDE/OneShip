# Chrome Extension ( Chrome API ) + React

## Folder Structure:

<pre>
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
</pre>

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
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import { resolve } from 'path';

export default defineConfig({
  // Define multiple entry points for background, popup, and options
  build: {
    rollupOptions: {
      input: {
        background: resolve(__dirname, 'src/background/background.ts'),
        content: resolve(__dirname, 'src/content/content.ts'),
        popup: resolve(__dirname, 'src/popup/popup.tsx'),
        options: resolve(__dirname, 'src/options/options.tsx'),
      },
      output: {
        entryFileNames: '[name]/[name].js',
        assetFileNames: 'assets/[name].[ext]',
      },
    },
    outDir: 'dist',  // Output directory for the built files
  },
  plugins: [
    react(),  // Enable React fast refresh
  ],
  resolve: {
    alias: {
      '@': resolve(__dirname, 'src'),  // Set up alias for `src/` directory
    },
  },
  server: {
    port: 3000,  // Vite development server port
    open: false,  // Don't open the browser automatically
    hmr: { overlay: true },  // Enable Hot Module Replacement
  },
})
```



* **Multiple Entry Points** : We have defined separate entry points for background scripts, content scripts, popup scripts, and options page scripts using Vite’s `rollupOptions.input`. Each of these entry points corresponds to a file in the `src/` folder.
* **Output Structure** : The `entryFileNames` configuration ensures that each script is placed in its own directory under `dist/`. The assets like images, fonts, and CSS are placed in the `assets/` folder.
* **React Fast Refresh** : The `react()` plugin enables fast refresh in React for a better development experience.
* **Alias** : You can set up an alias like `@` to shorten your import paths from the `src/` directory. This improves code readability and makes imports more convenient.
* **Server Configurations** : The `server` section configures Vite’s development server. It specifies the port, prevents automatic browser opening, and enables hot module replacement (HMR).

## Manifest.json

Here’s a complete guide to the `manifest.json` file for Chrome extensions using Manifest Version 3 (MV3). The `manifest.json` is the central configuration file for a Chrome extension, specifying permissions, background scripts, content scripts, and more.

### Basic Structure of `manifest.json`

```json
{
  "manifest_version": 3,
  "name": "My Chrome Extension",
  "version": "1.0.0",
  "description": "A description of the Chrome extension.",
  "icons": {
    "16": "icons/icon16.png",
    "48": "icons/icon48.png",
    "128": "icons/icon128.png"
  },
  "action": {
    "default_popup": "popup/popup.html",
    "default_icon": "icons/icon128.png"
  },
  "background": {
    "service_worker": "background.js"
  },
  "permissions": ["storage", "tabs"],
  "host_permissions": ["<all_urls>"],
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["content/content.js"],
      "css": ["content/styles.css"],
      "run_at": "document_idle"
    }
  ],
  "options_page": "options/options.html",
  "web_accessible_resources": [
    {
      "resources": ["content/inject.js", "icons/icon48.png"],
      "matches": ["<all_urls>"]
    }
  ]
}
```



### Key Components and Configurations

#### 1. **`manifest_version`**

- **Type**: Integer
- **Description**: Must be set to `3` for Manifest Version 3.

```json
   "manifest_version": 3
```

#### 2. **`name`**

- **Type**: String
- **Description**: The name of the Chrome extension.

```json
   "name": "My Chrome Extension"
```

#### 3. **`version`**

- **Type**: String
- **Description**: The version of the extension (following Semantic Versioning rules).

```json
   "version": "1.0.0"
```

#### 4. **`description`**

- **Type**: String
- **Description**: A short description of the extension.

```json
   "description": "A description of the Chrome extension."
```

#### 5. **`icons`**

- **Type**: Object
- **Description**: Specifies the icons to be used for the extension in different sizes.

```json
   "icons": {
     "16": "icons/icon16.png",
     "48": "icons/icon48.png",
     "128": "icons/icon128.png"
   }
```

#### 6. **`action`**

- **Type**: Object
- **Description**: Defines the default popup and icon for the extension's action button (the toolbar icon).

```json
   "action": {
     "default_popup": "popup/popup.html",
     "default_icon": "icons/icon128.png"
   }
```

#### 7. **`background`**

- **Type**: Object
- **Description**: Defines the background logic that runs when the extension is active. In Manifest V3, background pages are replaced by service workers.

```json
   "background": {
     "service_worker": "background.js"
   }
```

#### 8. **`permissions`**

- **Type**: Array of Strings
- **Description**: Specifies the permissions the extension needs to function.

```json
   "permissions": ["storage", "tabs"]
```

#### 9. **`host_permissions`**

- **Type**: Array of Strings
- **Description**: Grants the extension access to specific websites or all URLs (`<all_urls>`).

```json
   "host_permissions": ["https://example.com/*", "<all_urls>"]
```

#### 10. **`content_scripts`**

- **Type**: Array of Objects
- **Description**: Defines scripts that run in the context of a web page.

```json
   "content_scripts": [
     {
       "matches": ["https://example.com/*"],
       "js": ["content/content.js"],
       "css": ["content/styles.css"],
       "run_at": "document_idle"
     }
   ]
```

- **`matches`**: Specifies the URLs where the content script will be injected.
- **`js`**: Specifies the JavaScript files to inject.
- **`css`**: Specifies the CSS files to inject.
- **`run_at`**: Determines when the content script will be injected (e.g., `"document_idle"`, `"document_start"`).

#### 11. **`options_page`**

- **Type**: String
- **Description**: Points to an HTML page that is shown as the options page for the extension.

```json
   "options_page": "options/options.html"
```

#### 12. **`web_accessible_resources`**

- **Type**: Array of Objects
- **Description**: Lists resources that can be accessed by web pages or content scripts.

```json
   "web_accessible_resources": [
     {
       "resources": ["content/inject.js", "icons/icon48.png"],
       "matches": ["https://example.com/*"]
     }
   ]
```

#### 13. **`commands`**

- **Type**: Object
- **Description**: Allows the user to define keyboard shortcuts for the extension.

```json
   "commands": {
     "toggle-feature": {
       "suggested_key": {
         "default": "Ctrl+Shift+Y"
       },
       "description": "Toggle a feature"
     }
   }
```

#### 14. **`declarative_net_request`**

- **Type**: Object
- **Description**: Allows the extension to control web requests declaratively.

```json
   "declarative_net_request": {
     "rule_resources": [
       {
         "id": "ruleset_1",
         "enabled": true,
         "path": "rules.json"
       }
     ]
   }
```

#### 15. **`content_security_policy`**

- **Type**: Object
- **Description**: Specifies the security policy for the extension, ensuring the script's integrity.

```json
   "content_security_policy": {
     "extension_pages": "script-src 'self'; object-src 'self'"
   }
```

#### 16. **`chrome_url_overrides`**

- **Type**: Object
- **Description**: Allows the extension to override specific Chrome pages (e.g., the new tab page).

```json
   "chrome_url_overrides": {
     "newtab": "newtab.html"
   }
```

#### 17. **`omnibox`**

- **Type**: Object
- **Description**: Provides custom functionality in Chrome’s address bar.

```json
   "omnibox": {
     "keyword": "myextension"
   }
```

#### 18. **`background` (Alternative Configuration with Service Worker)**

- In Manifest V3, background scripts are now service workers.

```json
   "background": {
     "service_worker": "background.js"
   }
```

#### 19. **`side_panel`** (new feature in Manifest V3)

- **Type**: Object
- **Description**: Defines a side panel that can be opened from the Chrome toolbar.

```json
   "side_panel": {
     "default_path": "sidepanel.html"
   }
```

#### 20. **`minimum_chrome_version`**

- **Type**: String
- **Description**: Specifies the minimum required version of Chrome.

```json
   "minimum_chrome_version": "92"
```

### Example of a Full Manifest V3 Configuration

```json
{
  "manifest_version": 3,
  "name": "My Extension",
  "version": "1.0.0",
  "description": "A simple Chrome extension.",
  "icons": {
    "16": "icons/icon16.png",
    "48": "icons/icon48.png",
    "128": "icons/icon128.png"
  },
  "action": {
    "default_popup": "popup/popup.html",
    "default_icon": "icons/icon128.png"
  },
  "background": {
    "service_worker": "background.js"
  },
  "permissions": ["storage", "tabs"],
  "host_permissions": ["<all_urls>"],
  "content_scripts": [
    {
      "matches": ["https://*.example.com/*"],
      "js": ["content/content.js"],
      "css": ["content/styles.css"],
      "run_at": "document_idle"
    }
  ],
  "options_page": "options/options.html",
  "web_accessible_resources": [
    {
      "resources": ["content/inject.js", "icons/icon48.png"],
      "matches": ["https://*.example.com/*"]
    }
  ],
  "commands": {
    "toggle-feature": {
      "suggested_key": {
        "default": "Ctrl+Shift+Y"
      },
      "description": "Toggle a feature"
    }
  },
  "declarative_net_request": {
    "rule_resources": [
      {
     

 "id": "ruleset_1",
        "enabled": true,
        "path": "rules.json"
      }
    ]
  },
  "content_security_policy": {
    "extension_pages": "script-src 'self'; object-src 'self'"
  },
  "chrome_url_overrides": {
    "newtab": "newtab.html"
  },
  "omnibox": {
    "keyword": "myextension"
  },
  "side_panel": {
    "default_path": "sidepanel.html"
  },
  "minimum_chrome_version": "92"
}
```

This guide covers the essential configurations for Manifest Version 3 for Chrome extensions.
