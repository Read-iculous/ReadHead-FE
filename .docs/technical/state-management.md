# State Management — ReadHead-FE

## 1. State Classification Matrix

| Category | Storage / Tool | Examples | Scope & Lifetime |
|---|---|---|---|
| **Server State** | TanStack Query v5 | User profile, conversation list, BYOK key status, comprehension quiz history | Async, cached in-memory with automatic background stale revalidation |
| **Global Client State** | Zustand | Active `tabId`, active selection text, sidepanel drawer open/close, dark/light theme | Synchronous in-memory, reactive across components |
| **Persistent Local State** | `chrome.storage.local` / `localStorage` | JWT tokens, local unsent drafts, user offline settings, domain blacklist | Survives browser / extension restarts |
| **Session Cache** | `chrome.storage.session` / `sessionStorage` | Ephemeral tab DOM context, temporary pre-flight inspection cache | Cleared when browser session closes |
| **Local Component State** | `useState` / `useReducer` | Form input values, dropdown open states, animation timers | Component lifecycle only |

## 2. Zustand Store Definitions

### `useAuthStore`
```ts
interface AuthState {
  user: UserProfile | null;
  isAuthenticated: boolean;
  isLoading: boolean;
  setAuth: (user: UserProfile, tokens: AuthTokens) => void;
  logout: () => void;
}
```

### `useTabContextStore`
```ts
interface TabContextState {
  activeTabId: number | null;
  activeUrl: string;
  extractedArticle: ExtractedArticle | null;
  selectedText: string;
  activeDraft: string;
  setActiveTab: (tabId: number, url: string) => void;
  setExtractedArticle: (article: ExtractedArticle) => void;
  setSelectedText: (text: string) => void;
  setActiveDraft: (draft: string) => void;
}
```

### `useSettingsStore`
```ts
interface AccessibilityProfile {
  dyslexia_font: boolean;
  font_size_delta: number;
  line_spacing: number;
  bionic_reading: boolean;
  high_contrast: boolean;
}

interface SettingsState {
  default_provider: 'gemini' | 'openai' | 'claude';
  default_model: string;
  target_language: string;
  explanation_level: 'simple' | 'standard' | 'expert';
  proactive_assistance_enabled: boolean;
  proactive_sensitivity: 'low' | 'medium' | 'high';
  data_protection_mode: boolean;
  privacy_mode_enabled: boolean;
  accessibility_profile: AccessibilityProfile;
  updateSettings: (partial: Partial<SettingsState>) => void;
}
```
