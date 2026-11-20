# Setup & Installation — ReadHead-FE

## 1. Prerequisites
- **Node.js**: `v20.x` or `v22.x` LTS recommended.
- **Package Manager**: `pnpm` (preferred) or `npm`.
- **Google Chrome**: Version 116+ (Required for `chrome.sidePanel` API support).
- **Backend Running**: `ReadHead-BE` running locally on `http://localhost:8000`.

---

## 2. Initial Setup
```bash
# Clone the repository and navigate into FE
cd ReadHead-FE

# Install dependencies
pnpm install

# Create local environment config
cp .env.example .env.development
```

Ensure `.env.development` contains:
```env
VITE_API_BASE_URL=http://localhost:8000
VITE_AI_STREAM_ENDPOINT=/api/v1/ai/stream
VITE_APP_ENV=development
```

---

## 3. Running Development Server

### 3.1 Start Vite Development Server
```bash
pnpm run dev
```
This builds both the Web Portal (accessible at `http://localhost:5173`) and outputs the Chrome Extension build to `dist/`.

---

## 4. Loading the Chrome Extension in Browser
1. Open Google Chrome and navigate to `chrome://extensions/`.
2. Toggle **Developer mode** (switch in top-right corner) to **ON**.
3. Click the **Load unpacked** button.
4. Select the `dist/` directory inside `ReadHead-FE`.
5. The **ReadHead — Intelligent Reading Assistant** icon will appear in your Chrome toolbar.
6. Pin the extension for quick access.
7. Any changes in `src/` will trigger Hot Module Replacement (HMR) or prompt a quick extension reload.
