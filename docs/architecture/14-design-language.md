# 14 - Design Language & Tokens — Support Copilot · Nhà Gọn

> **Status:** Active  
> **Brand:** Support Copilot · Nhà Gọn  
> **Source Demo Reference:** `https://assets.200lab.io/static/mini-solutions/support-copilot-sidebar/demo/bf2ed14/index.html?replay=1&autoplay=1&view=split`  
> **Last Updated:** 2026-10-12  

---

## 1. Design Token Architecture

Hệ thống token tuân theo mô hình 3 tầng:
$$\text{Primitive Tokens (Raw Values)} \longrightarrow \text{Semantic Tokens (Purpose Aliases)} \longrightarrow \text{Component Tokens}$$

---

## 2. Token Definitions

### 2.1. Typography Tokens
```css
:root {
  /* Font Family */
  --font-family-primary: 'Be Vietnam Pro', ui-sans-serif, system-ui, sans-serif;
  --font-family-mono: 'JetBrains Mono', 'Fira Code', ui-monospace, monospace;

  /* Font Scale */
  --font-size-xs: 12px;
  --font-size-sm: 13px;
  --font-size-base: 14px;
  --font-size-lg: 15px;
  --font-size-xl: 16px;

  /* Line Heights */
  --font-line-height-xs: 18px;
  --font-line-height-sm: 19.5px;
  --font-line-height-base: 21px;
  --font-line-height-lg: 22.5px;
  --font-line-height-xl: 24px;

  /* Weights */
  --font-weight-regular: 400;
  --font-weight-medium: 500;
  --font-weight-semibold: 600;
  --font-weight-bold: 700;
}
```

### 2.2. Color Tokens (OKLCH Dark Surfaces)
```css
:root {
  /* Text */
  --color-text-primary: oklch(0.965 0.006 262);
  --color-text-secondary: oklch(0.82 0.016 262);
  --color-text-tertiary: oklch(1 0 0);
  --color-text-inverse: oklch(0.72 0.02 262);

  /* Surface */
  --color-surface-base: #000000;
  --color-surface-strong: oklch(0.155 0.02 262);
  --color-surface-muted: oklch(0.215 0.024 262);
  --color-surface-raised: oklch(0.255 0.026 262);

  /* Border */
  --color-border-muted: oklch(0.3 0.024 262);
  --color-border-focus: oklch(0.85 0.15 145);

  /* Brand & Accents */
  --color-accent-brand: oklch(0.76 0.18 145);
  --color-accent-warning: oklch(0.70 0.19 50);
  --color-accent-agent: oklch(0.68 0.20 280);
}
```

### 2.3. Spacing Tokens
```css
:root {
  --space-1: 6px;
  --space-2: 8px;
  --space-3: 10px;
  --space-4: 12px;
  --space-5: 14px;
  --space-6: 16px;
  --space-7: 56.09px;
}
```

### 2.4. Radius & Motion Tokens
```css
:root {
  --radius-xs: 6px;
  --radius-sm: 8px;
  --radius-md: 12px;
  --radius-lg: 16px;
  --radius-xl: 26843500px; /* Full pill */

  --motion-duration-instant: 150ms;
  --motion-timing-standard: cubic-bezier(0.16, 1, 0.3, 1);
}
```
