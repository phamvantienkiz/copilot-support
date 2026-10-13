# UI Design System & Token Guidance — Support Copilot · Nhà Gọn

> **Status:** Active  
> **Brand / Surface:** Support Copilot · Nhà Gọn (E-commerce Customer Support Surface)  
> **Source Demo:** `https://assets.200lab.io/static/mini-solutions/support-copilot-sidebar/demo/bf2ed14/index.html?replay=1&autoplay=1&view=split`  
> **Accessibility Target:** WCAG 2.2 AA  
> **Last Verified:** 2026-10-12  

---

## 1. Design Intent & Context

**Design Intent:** Provide an implementation-ready, deterministic, token-driven interface for human operators and shoppers that minimizes cognitive load during high-friction support disputes through clear information density, keyboard-first accessibility, and transparent AI reasoning.

### Context & Goals
- **Product Surface:** E-commerce Customer Support Workspace (Split View: Storefront Shopper Simulator, Operator Console, and Support Copilot AI Drawer).
- **Core Audience:** Customer service operators handling multi-order disputes and online consumers seeking fast resolution.
- **Primary Goals:**
  1. Maintain 100% visual and token consistency across all 3 viewports (`customer`, `operator`, `split`).
  2. Guarantee WCAG 2.2 AA compliance across all dark-surface interactions.
  3. Ensure zero ambiguous actions: operators must be able to inspect, select, edit, and dispatch responses within seconds using keyboard shortcuts.

---

## 2. Design Tokens & Foundations

All interface styling must use semantic tokens. Raw hex values, arbitrary pixel spacings, or untokenized inline styles are strictly prohibited.

### 2.1. Typography Foundations
- **Primary Font Family:** `Be Vietnam Pro`
- **Font Family Stack:** `Be Vietnam Pro, ui-sans-serif, system-ui, sans-serif`
- **Base Metrics:**
  - `font.size.base`: `14px`
  - `font.weight.base`: `400`
  - `font.lineHeight.base`: `21px`

#### Typography Scale
| Token | Computed Size | Line Height | Recommended Usage |
| :--- | :--- | :--- | :--- |
| `font.size.xs` | `12px` | `18px` | Timestamps, token counters, badge metadata |
| `font.size.sm` | `13px` | `19.5px` | Secondary descriptors, tool execution times, metric labels |
| `font.size.md` | `14px` | `21px` | Message body text, suggestion text, input contents |
| `font.size.lg` | `15px` | `22.5px` | Section titles, customer names, primary tab labels |
| `font.size.xl` | `16px` | `24px` | Main headers, insight alert headlines |

### 2.2. Color Tokens (OKLCH & Dark Surfaces)
Color tokens are specified in `oklch` for uniform perceptual lightness and high contrast on dark OLED/LCD screens.

```css
:root {
  /* Text Tokens */
  --color-text-primary: oklch(0.965 0.006 262);    /* Primary high-contrast text */
  --color-text-secondary: oklch(0.82 0.016 262);   /* Secondary / subtitle text */
  --color-text-tertiary: oklch(1 0 0);             /* Pure white highlight text */
  --color-text-inverse: oklch(0.72 0.02 262);      /* Muted / placeholder / inactive text */

  /* Surface Tokens */
  --color-surface-base: #000000;                   /* Deep black backdrop */
  --color-surface-strong: oklch(0.155 0.02 262);   /* Column panels & drawer container */
  --color-surface-muted: oklch(0.215 0.024 262);    /* Message bubbles, input wrappers */
  --color-surface-raised: oklch(0.255 0.026 262);   /* Hovered cards, suggestion item surfaces */

  /* Border Tokens */
  --color-border-muted: oklch(0.3 0.024 262);     /* Hairline panel dividers and card borders */
  --color-border-focus: oklch(0.85 0.15 145);      /* High-visibility focus indicator */

  /* Accent & Status Tokens */
  --color-accent-brand: oklch(0.76 0.18 145);      /* Green primary action: "Sử dụng", "Gửi" */
  --color-accent-warning: oklch(0.70 0.19 50);     /* Warning: "Giao trễ", "Frustrated" badge */
  --color-accent-agent: oklch(0.68 0.20 280);       /* Agent active processing / thinking */
}
```

### 2.3. Spacing Scale
| Token | Value | Strict Usage |
| :--- | :--- | :--- |
| `space.1` | `6px` | Badge padding, micro tag gaps, inline icon margins |
| `space.2` | `8px` | Button inline padding, internal card item gap |
| `space.3` | `10px` | List item gaps, compact input vertical padding |
| `space.4` | `12px` | Card internal padding, container gutter |
| `space.5` | `14px` | Chat bubble padding, operator textarea padding |
| `space.6` | `16px` | Section margins, column headers, card group padding |
| `space.7` | `56.09px` | Main header height, fixed action toolbar anchor |

### 2.4. Radius, Shadows & Motion
- `radius.xs`: `6px` (Status badges, micro tags)
- `radius.sm`: `8px` (Buttons, input boxes, tool trace pills)
- `radius.md`: `12px` (Suggestion cards, customer profile summary card)
- `radius.lg`: `16px` (Chat bubble envelopes, main modal panels)
- `radius.xl`: `26843500px` (Full pill: view switcher toggles, avatar badges)
- `motion.duration.instant`: `150ms ease-out` (Transitions: hover, focus, tab switch)

---

## 3. Component-Level Rules & Specifications

### Page Component Density Overview
- **Buttons (32 instances):** Primary CTAs ("Sử dụng", "Gửi"), tone switchers, scenario selectors, replay toggles, theme toggles, modal triggers.
- **Cards (10 instances):** 3 Suggestion Cards, 1 Customer Profile Card, 1 AI Insight Card, 4 Tool Step Trace Cards, 1 Delivery Timeline Card.
- **Navigation (6 instances):** View Switcher (Khách hàng, Người trực, Song song), Copilot Tabs (Agent, Single call, Hồ sơ nhanh).
- **Lists (6 instances):** Chat messages (Shopper), Chat messages (Operator), Tool trace items, Order history items, Sample scenarios list, Suggestion list.
- **Inputs (3 instances):** Shopper chat input, Operator response textarea, Scenario search filter.

---

### 3.1. Buttons (`Button`)

#### Anatomy
`[Leading Icon (optional)] + [Text Label] + [Trailing Shortcut Badge (optional)]`

#### Variants
1. **Primary Action ("Sử dụng", "Gửi"):**
   - Background: `var(--color-accent-brand)`
   - Text: `#000000` (High-contrast dark on bright green)
   - Radius: `radius.sm` (`8px`)
   - Font: `font.size.md` (`14px`), `font.weight.base` (`500`)
2. **Secondary / Utility (Tone badge, Copy action):**
   - Background: `var(--color-surface-raised)`
   - Border: `1px solid var(--color-border-muted)`
   - Text: `var(--color-text-primary)`
3. **Ghost / Tab Button (Header view switcher):**
   - Background: `transparent` (Active: `var(--color-surface-raised)`)
   - Text: `var(--color-text-secondary)` (Active: `var(--color-text-tertiary)`)

#### State Behavior Matrix
| State | Visual Treatment | Pointer / Cursor | Keyboard |
| :--- | :--- | :--- | :--- |
| **Default** | Standard token styling | `pointer` | Focusable via `Tab` |
| **Hover** | Background brightness +10%, `motion.duration.instant` | `pointer` | N/A |
| **Focus-Visible** | Outline: `2px solid var(--color-border-focus)`, offset `2px` | `pointer` | `Enter` / `Space` activates |
| **Active** | Scale transform: `0.98`, background brightness -5% | `pointer` | Trigger action |
| **Disabled** | Opacity: `0.4`, background: `var(--color-surface-muted)` | `not-allowed` | Skipped in tab order |
| **Loading** | Spinner replaces leading icon; label persists | `wait` | Disabled until resolved |
| **Error** | Border: `1px solid var(--color-accent-warning)` | `pointer` | Error tooltip displayed |

---

### 3.2. Cards (`SuggestionCard`, `CustomerProfileCard`, `TraceCard`)

#### Anatomy
`[Card Header: Status Badge + Title + Action] + [Card Body: Structured Text / Metrics] + [Card Footer (optional)]`

#### Rules
- **Spacing:** Internal padding must be `space.4` (`12px`) for compact trace items, `space.5` (`14px`) for suggestion cards.
- **Border:** Must have a constant `1px solid var(--color-border-muted)` to maintain shape on pitch-black surfaces.
- **Long Content Handling:**
  - Suggestion text must wrap naturally with `line-height: 21px`.
  - If text exceeds 6 lines, it should display full text inside an auto-scroll container or preserve fixed height without breaking layout.
- **Empty State:**
  - When no suggestions are generated, card must display an explicit state: *"Không có gợi ý khả dụng cho hội thoại này"* with a manual retry button.

---

### 3.3. Navigation & Tab Switchers (`ViewSwitcher`, `CopilotTabs`)

#### Anatomy
`Container (radius.xl pill, background: var(--color-surface-muted))` containing `Tab Items (radius.xl)`.

#### Keyboard & Interaction
- Must support `ArrowLeft` / `ArrowRight` navigation within tab lists (`role="tablist"`).
- Selected tab must have `aria-selected="true"`.
- Tab change must execute within `motion.duration.instant` without reloading state.

---

### 3.4. Inputs & Textareas (`ChatInput`, `OperatorTextarea`)

#### Anatomy
`[Textarea Surface] + [Bottom Bar: Keyboard Shortcut Indicator + Send Button]`

#### Rules & States
- **Default:** Background: `var(--color-surface-muted)`, Border: `1px solid var(--color-border-muted)`, Text: `var(--color-text-primary)`.
- **Focus-Visible:** Border: `1px solid var(--color-border-focus)`, Box-shadow: `0 0 0 1px var(--color-border-focus)`.
- **Shortcuts:**
  - `Enter`: Submit / Send message.
  - `Shift + Enter`: Newline.
- **Overflow:** Auto-expands from 1 line (`42px`) to max 6 lines (`140px`) before vertical scrolling triggers.
- **Empty State:** Placeholder text must use `var(--color-text-inverse)`: *"Nhắn tin cho Nhà Gọn..."* or *"Trả lời [Tên khách]..."*.

---

### 3.5. Thanh Phân rã Điểm kép (`DualScoreBar`)

#### Anatomy
`[Label & Title] + [Stacked Progress Bar (BM25: Grey/Slate + Semantic: Cyan/Blue)] + [Score Value: 0.00 -> 1.00]`

#### Rules & Tokens
- **Bar Container:** Height `6px`, Radius `radius.full`, Background `oklch(0.2 0.01 262)`.
- **BM25 Segment (Từ khóa):** Width tỷ lệ theo điểm BM25, Background `oklch(0.6 0.02 262)` (Xám / Slate).
- **Semantic Segment (Ý nghĩa):** Width tỷ lệ theo điểm Semantic, Background `oklch(0.65 0.15 220)` (Xanh cyan / Sky blue).
- **Tooltip On Hover:** Hiển thị chi tiết: `BM25: {score} | Semantic: {score} => Final: {weighted_score}`.

---

### 3.6. Biểu đồ Cột 3 Đơn Gần Nhất (`OrderTimelineBarChart`)

#### Anatomy
`[Chart Header: "3 ĐƠN GẦN NHẤT" + Range "05 -> 09/2026"] + [3 Vertical Bars] + [Bar Labels: Tên sản phẩm, Mã đơn, Ngày, Ghi chú trễ] + [Legend]`

#### Rules & States
- **Cột Đúng hẹn:** Màu Cyan (`oklch(0.75 0.14 200)`), Ghi chú: "đúng hẹn".
- **Cột Trễ hẹn:** Màu Vàng (`oklch(0.8 0.16 85)`), Ghi chú: "trễ {N} ngày".
- **Cột Đang khiếu nại:** Nền sọc chéo Vàng + Viền Đỏ (`oklch(0.65 0.22 25)`), Bar outline nổi bật.
- **Giá trị đơn hàng:** Hiển thị số tiền phía trên đầu cột (vd: `420k`, `660k`, `1,44tr`).

---

### 3.7. Thẻ Cảnh báo Tiền lệ ("CẦN LƯU Ý" - `ContextWarningCard`)

#### Anatomy
`[Header: Icon Alert đỏ/cam + Title "CẦN LƯU Ý" + Badge "AI · từ context"] + [Body: Cảnh báo sự cố lặp lại] + [Footer: Tag Chips]`

#### Rules & Tokens
- **Surface:** Background `oklch(0.18 0.03 262)`, Border `1px solid oklch(0.4 0.1 30)`.
- **Tag Chips:** Radius `radius.xs`, Background `oklch(0.25 0.02 262)`, Text `oklch(0.8 0.02 262)`.
- **Interactive:** Nhấp vào tag `#10006` mở modal/drawer xem chi tiết đơn hàng quá khứ.

---

### 3.8. Thẻ Nhận định AI (`AiAssessmentCard`)

#### Anatomy
`[Icon Sparkles + Title "NHẬN ĐỊNH AI"] + [Headline: "Lỗi kép: ..."] + [Paragraph: Phân tích bối cảnh và khuyến nghị]`

#### Rules & Tokens
- **Headline Font:** `font.size.lg`, `font.weight.bold`, Color `oklch(0.95 0.02 262)`.
- **Accent Indicator:** Đường viền dọc bên trái màu đỏ cam hoặc tím nhạt để tạo điểm nhấn thị giác.

## 4. Accessibility Requirements & Acceptance Criteria (WCAG 2.2 AA)

### 4.1. Contrast Constraints
- Text (`var(--color-text-primary)`) on surfaces (`var(--color-surface-base)`, `var(--color-surface-muted)`) must exceed **7:1** contrast ratio (exceeds WCAG AAA).
- Secondary text (`var(--color-text-secondary)`) must exceed **4.5:1** contrast ratio.
- Interactive boundaries and focus rings must exceed **3:1** against adjacent dark backgrounds.

### 4.2. Keyboard-First Navigation
- The entire workflow from reading customer message to dispatching response must be completable without a mouse:
  1. `Tab` to navigate to the desired suggestion card.
  2. `Space` or `Enter` on "Sử dụng" to transfer drafted text into the operator input.
  3. Focus automatically moves into the input box.
  4. Operator makes edits and presses `Enter` to dispatch.

### 4.3. Testable Acceptance Criteria
| Check ID | Requirement | Test Condition | Pass/Fail Criteria |
| :---: | :--- | :--- | :--- |
| **A11Y-01** | Focus Visibility | Navigate via `Tab` across all 32 buttons and inputs | **PASS:** High-contrast focus outline visible on 100% of focused elements. |
| **A11Y-02** | Screen Reader Live Trace | Agent turns progress (vòng 1, 2, tool calls) | **PASS:** `aria-live="polite"` announces agent step updates without interrupting user. |
| **A11Y-03** | Accessible Names | Icon-only buttons (Replay, Theme, Close) | **PASS:** `aria-label` present and descriptive on all icon buttons. |
| **A11Y-04** | Touch Target Size | Touch interactions on tablets/touchscreens | **PASS:** All clickable targets have minimum size of `44px x 44px`. |

---

## 5. Content & Tone Standards

The Copilot UI generates responses across **3 discrete customer service tones**:

### 5.1. Tone Specifications & Examples
1. **Empathetic (Thấu cảm):**
   - *Target:* Khách hàng có trạng thái tức giận (`Frustrated`), gặp lỗi kép (vừa giao trễ vừa hỏng đồ).
   - *Standard:* Lập tức nhận trách nhiệm ở câu đầu tiên, không đổ lỗi bên thứ ba, cam kết đổi mới và đề xuất bồi thường.
   - *Example:* *"Dạ shop thật sự xin lỗi anh, đơn máy xay sinh tố Zenka B600 vừa giao trễ 4 ngày lại còn bị nứt cối thủy tinh, đây là lỗi của shop..."*
2. **Concise (Ngắn gọn):**
   - *Target:* Khách hàng cần phương án giải quyết nhanh, hỏi thủ tục hoàn tiền/đổi trả.
   - *Standard:* Nêu trực tiếp mã đơn, thời hạn và hành động cụ thể trong 2-3 câu.
   - *Example:* *"Dạ shop xin lỗi anh vì đơn #10032 giao trễ 4 ngày và cối thủy tinh bị nứt. Anh gửi shop ảnh cối nứt và tem vận chuyển, shop đổi máy mới trong 7 ngày..."*
3. **Formal (Trang trọng):**
   - *Target:* Khách hàng doanh nghiệp hoặc yêu cầu xác nhận văn bản theo quy chuẩn cửa hàng.
   - *Standard:* Xưng hô chuẩn thương hiệu ("Nhà Gọn"), ngôn từ chuyên nghiệp.
   - *Example:* *"Dạ Nhà Gọn thành thật xin lỗi anh vì đơn máy xay sinh tố giao trễ 4 ngày và cối thủy tinh bị nứt khi mở hộp, shop xác nhận đây là lỗi của shop..."*

---

## 6. Anti-Patterns & Prohibited Implementations

- ❌ **Prohibited:** Using raw hex colors (`#1a1a1a`, `#22c55e`) inside React components or Tailwind classes. All styles must reference design tokens.
- ❌ **Prohibited:** Silent or automatic sending. Copilot must **never** send a message directly to the customer without human operator confirmation.
- ❌ **Prohibited:** Hiding focus rings (`outline: none` without replacement).
- ❌ **Prohibited:** One-off margin/padding values (e.g. `margin: 13px` or `padding: 7px`). Must strictly snap to the `space.*` scale.
- ❌ **Prohibited:** Unhandled overflow in suggestion cards that clips text or pushes input bars off-screen.

---

## 7. QA & Implementation Checklist

Before any UI pull request is merged, verify every item:

- [ ] **Token Audit:** Are 100% of CSS rules mapped to `--color-*`, `--space-*`, `--radius-*`, and `--font-*` tokens?
- [ ] **Component States:** Does every button, input, and card implement all 7 states (Default, Hover, Focus-visible, Active, Disabled, Loading, Error)?
- [ ] **3-View Responsiveness:** Does the split view cleanly collapse to single-column on viewport width `< 1200px`?
- [ ] **Density Check:** Are all 32 buttons, 10 cards, 6 navigation elements, 6 lists, and 3 inputs rendering correctly?
- [ ] **Keyboard Flow:** Can an operator use `Tab` $\rightarrow$ select suggestion $\rightarrow$ edit $\rightarrow$ press `Enter` to send without touching the mouse?
- [ ] **Contrast Check:** Have contrast ratios been verified with browser devtools (WCAG AA pass)?
- [ ] **Empty & Error States:** Do message lists, trace panels, and suggestion boxes show clean fallbacks when data is empty or API fails?
