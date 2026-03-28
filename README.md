# Pattern Chrome Extension

Your personal stylist — surfaces the right products for your taste on any brand site.

## Setup

### 1. Add your credentials

**Supabase** — open `src/supabase.js` and replace:
```
const SUPABASE_URL = "YOUR_SUPABASE_URL"
const SUPABASE_ANON_KEY = "YOUR_SUPABASE_ANON_KEY"
```

**Anthropic API** — the Claude API calls in `src/claude.js` use the proxy already configured. No key needed in the extension itself — add your key to the proxy if you have one set up, otherwise add it directly to the headers in `src/claude.js`:
```js
"x-api-key": "YOUR_ANTHROPIC_API_KEY",
"anthropic-version": "2023-06-01",
```

### 2. Add icons
Drop three PNG files into `/icons/`:
- `icon16.png` (16×16)
- `icon48.png` (48×48)  
- `icon128.png` (128×128)

A simple black "P" on white background works fine for the demo.

### 3. Load the extension in Chrome
1. Open Chrome → `chrome://extensions`
2. Enable **Developer mode** (top right toggle)
3. Click **Load unpacked**
4. Select the `pattern-extension` folder
5. The Pattern icon appears in your toolbar

### 4. Make sure your Supabase profiles table has these fields
The extension expects the profile row to have:
- `email`
- `archetype` (string — e.g. "The Minimalist")
- `style_words` (array — e.g. ["delicate", "understated", "architectural"])
- `color_palette` (array — e.g. ["ivory", "charcoal", "french blue"])
- `brand_affinities` (array — e.g. ["Acne Studios", "Catbird", "Toteme"])

Adjust field names in `src/supabase.js` if yours differ.

## How it works

1. Enter your email → pulls your Pattern Taste ID from Supabase
2. On any brand site, open the panel and describe what you're looking for
3. Claude picks which collection pages to crawl based on your intent
4. Extension crawls those pages and extracts products
5. Local filter removes anything over budget or irrelevant
6. Claude ranks the survivors against your taste profile with a reason per item
7. Click any product → opens in main window, panel stays open

## File structure

```
pattern-extension/
├── manifest.json          # Chrome extension config
├── sidepanel.html         # Side panel UI shell
├── sidepanel.css          # Pattern editorial styles
├── sidepanel.js           # Main orchestration logic
├── icons/                 # Extension icons
└── src/
    ├── background.js      # Opens side panel on click
    ├── content.js         # Crawls product pages
    ├── supabase.js        # Profile fetch by email
    ├── claude.js          # Two Claude API calls
    └── filter.js          # Local budget + keyword filter
```

## Demo sites

Works out of the box on:
- `catbird-nyc.com` (Catbird-specific selectors)
- Any standard Shopify store (generic fallback)
