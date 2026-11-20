# بوابة اللغة الصينية - المستوى الأول

A Neo-Chinese Traditional, strictly Arabic (RTL) and Mandarin Chinese practice platform for Level 1 learners across 15 chapters, featuring teacher-managed student access, 9 interactive drill engines, Practice and Exam modes, active study-time analytics, and automated Google Sheets teacher sync.

---

## User Review & Critical Decisions

> [!IMPORTANT]
> All four product decisions from Phase 1 are locked into this plan. Please review the architecture and design below before approving execution.

- **Confirmed Brand Identity**: `بوابة اللغة الصينية - المستوى الأول` across the header, login gate, and page metadata.
- **Confirmed Language Purity**: Strictly Arabic (RTL) for all UI labels, analytics, and meanings alongside Mandarin Chinese (Hanzi + Pinyin). Zero English words or Latin exam codes (such as "HSK 3.0") anywhere in the rendered interface; Arabic terminology (`المستوى الأول · الإصدار الثالث`) is used exclusively.
- **Confirmed Global Pinyin Behavior**: Visible by default (`إظهار البينين`), toggled globally across all views, cards, and drills, while remaining automatically visible inside Pinyin-assessment drills (Hanzi → Pinyin and 3-Column Matching).
- **Confirmed Exam Mode Navigation**: Free forward/backward navigation and question jumping in `وضع الامتحان` so students can review or change any response before pressing final submit (`تسليم الامتحان`).
- **Curriculum Shell Scope**: Chapters 1–15 are scaffolded with strict TypeScript data contracts; Chapters 2–15 intentionally remain empty shells (`محتوى هذا الفصل قيد الإضافة`) while Chapter 1 holds 3 sample words and 2 sample sentences solely to validate every drill engine and UI flow.

---

## 1. Overview & Core Concept

- **What It Does**: Provides an end-to-end Mandarin Chinese practice studio tailored for Arabic-speaking beginners. Students authenticate with teacher-issued credentials, track progress across 15 structured chapters, train across 9 specialized drill engines (vocabulary, tones/Pinyin, matching, speech-synthesis listening, sentence translation, tile-based sentence assembly, and cloze completion), and review detailed performance analytics that automatically report to a teacher's Google Sheet.
- **Target Audience / Persona**: Arabic-speaking Level 1 Mandarin learners and their language instructor who manages student access codes and monitors aggregate progress remotely.
- **Key Value**: Combines distraction-free Neo-Chinese editorial craft with instant formative feedback (`وضع التدريب`) and realistic summative testing (`وضع الامتحان`), while requiring zero backend server hosting or external audio APIs.

---

## 2. User Experience & Visual Design

### Key User Flows

1. **Authentication Gate**: Unauthenticated visitors land on a tranquil Neo-Chinese login screen framed with traditional ink-wash imagery and cinnabar seal accents. Students enter their Arabic name and access code (with two ready test accounts documented for immediate teacher verification). Valid sessions persist in `localStorage`.
2. **Welcome Home View**: Greets the signed-in student by name, highlights their current streak, accuracy, and active study time in a clean horizontal summary strip, and centers a single dominant CTA (`ابدأ التدريب`) to enter the 15-chapter grid.
3. **15-Chapter Grid View**: Displays Chapters 1 through 15 with traditional Chinese numeral medallions (`一` to `十五`), Arabic chapter titles, item counts, and completion progress bars. Selecting an empty chapter displays an unhurried Arabic empty-state view (`محتوى هذا الفصل قيد الإضافة`) with a clear return action.
4. **Chapter Practice Hub**: For populated chapters, presents a segmented mode selector (`وضع التدريب` vs. `وضع الامتحان`), a prominent Mixed Quiz option (`اختبار شامل`), and a structured grid of all 9 individual drill engines with per-drill availability counts.
5. **Active Drill & Review Stage**:
   - In **Practice Mode (`وضع التدريب`)**, every answer triggers immediate dual-coded feedback (Jade Green + checkmark for correct, Cinnabar Red + cross with the right answer revealed for incorrect) before advancing.
   - In **Exam Mode (`وضع الامتحان`)**, students navigate freely across a question index bar, modify choices at will, and submit at the end to receive their score, accuracy breakdown, and full question-by-question answer review.
   - Quiz completion updates local analytics and silently triggers the Google Sheets sync payload.
6. **Student Profile & Analytics (`صفحتي / الإحصائيات`)**: Displays core KPIs (Total Solved, Correct, Wrong, Accuracy %, Active Study Time tracked only while the tab is visible, Overall 15-Chapter Progress %, Study Streak in days, and Weakest Drill Type) followed by a per-chapter breakdown table with accuracy and progress bars for all 15 chapters.

### Visual Identity & Theme

- **Aesthetic Direction**: Neo-Chinese Traditional Editorial ("Rice Paper & Night Ink") — calm, scholarly, and tactile. Crisp Arabic RTL composition paired with large calligraphic Hanzi specimens, subtle double-line corner ornaments, and vermilion seal accents. Zero cluttered pill badges, zero decorative pulsing dots, and strict 60-30-10 color restraint.
- **Color Palette & Mood** (Dual-Theme via Tailwind v4 `@custom-variant dark` and semantic CSS custom properties):
  - **Light Mode (Rice Paper / ورق الأرز)**:
    - `--background`: `#F9F6F0` (60% warm rice-paper canvas)
    - `--surface`: `#FFFFFF` (30% elevated paper cards with hairline ink borders)
    - `--foreground`: `#1C1917` (deep sumi ink typography)
    - `--primary`: `#B91C1C` (10% Cinnabar Red primary CTAs & active indicators)
    - `--accent`: `#D97706` (Imperial Gold streaks, milestones, and active highlights)
    - `--success`: `#15803D` (Jade Green mastery & correct responses)
  - **Dark Mode (Night Ink / حبر الليل)**:
    - `--background`: `#121110` (deep charcoal inkstone canvas)
    - `--surface`: `#1E1C1A` (elevated dark ink cards with warm hairline borders)
    - `--foreground`: `#F5F2EB` (soft rice-paper text with optical weight balance)
    - `--primary`: `#EF4444` (Bright Cinnabar)
    - `--accent`: `#F59E0B` (Luminous Gold)
    - `--success`: `#22C55E` (Bright Jade)
- **Typography & Hierarchy**:
  - **Arabic UI & Meanings**: `Cairo` (headings/UI controls) paired with `Tajawal` (body/meanings), strictly RTL (`dir="rtl"`), with `tabular-nums` for every metric, percentage, and timer readout.
  - **Mandarin Hanzi & Pinyin**: Large crisp `Noto Serif SC` (with `KaiTi` / `STKaiti` fallbacks) for Hanzi characters (`text-4xl`–`text-6xl` on drill stages) so stroke structure is effortlessly legible, paired with clean tone-diacritic Pinyin typography underneath when visible.
- **Component Styling & Layout**:
  - Single-row RTL Top Navigation Bar holding the brand wordmark (`بوابة اللغة الصينية - المستوى الأول`), clean text navigation links (`الرئيسية`, `الفصول`, `إحصائياتي`), and global utility controls (Pinyin visibility toggle, Light/Dark mode switch, and Logout button) kept strictly single-line with responsive mobile menu support.
  - Bespoke ink-wash botanical/mountain editorial artwork generated via `generate_image` for the Login and Welcome hero panels (with resilient SVG/CSS fallback containers).
- **Interactive Feedback & Motion**: Subtle compositor-only transitions (`opacity`, `transform`, $\le 200\text{ms}$) via `motion` for screen changes, tile assembly in the Sentence Builder, and matching pair locks, respecting `prefers-reduced-motion`. All semantic states pair color with explicit Arabic labels and clear icons.

---

## 3. Key Product Decisions & Trade-Offs

- **Small-Dataset Resilience Across All 9 Drills**
  - *Chosen Approach*: Each drill engine dynamically adapts its option count and distractor synthesis to the available chapter data. For Drill 3 (Hanzi → Pinyin), a dedicated tone-mutation and similar-syllable generator programmatically creates 2 tone variants and 2 phonetic distractors per word so 5 choices always render even with only 3 words in Chapter 1. For multiple-choice and Fill-in-the-Blank drills, if a chapter has fewer than 4 items, a tiny built-in fallback distractor list ensures clean 4-option questions without duplicates or crashes.
  - *Why*: Guarantees Chapter 1's 3 dummy words and 2 dummy sentences exercise every single drill engine smoothly on day one, while scaling seamlessly once the teacher populates full chapters.
  - *Alternatives Considered*: Crashing or locking drills until a minimum item count is reached — rejected because it would prevent verifying all 9 engines out of the box.

- **Client-Side Persistence + Apps Script Webhook Sync**
  - *Chosen Approach*: Persist student session, per-chapter mastery, per-drill accuracy history, daily study streak, and visibility-aware active study minutes per student ID in `localStorage`. On quiz completion, fire a `no-cors` JSON `POST` to the configured Google Apps Script Web App URL (falling back safely to local storage when the URL is empty).
  - *Why*: Matches the requested zero-backend architecture, works offline/instantly, and lets the teacher paste the provided `doPost(e)` script into any Google Sheet in minutes.
  - *Alternatives Considered*: Adding a hosted database or OAuth flow — unnecessary overhead given the static student roster and Apps Script requirement.

- **Visibility-Aware Active Study Timer**
  - *Chosen Approach*: A custom React hook listening to `document.visibilitychange`, `window.focus`, and `window.blur` increments active study seconds only while the student is authenticated and actively viewing the tab, flushing accumulated minutes cleanly to `localStorage`.
  - *Why*: Prevents inflated study-time analytics when students leave a browser tab open in the background.

---

## 4. Technical Architecture & Data Strategy *(Technical Reference)*

### System & Component Architecture

```text
┌─────────────────────────────────────────────────────────────────────────┐
│                    Global Contexts & Persistence Layer                  │
│  ┌──────────────┐  ┌────────────────┐  ┌───────────────┐  ┌──────────┐  │
│  │ Auth Context │  │ Settings Ctx   │  │ Progress &    │  │ Active   │  │
│  │ (localStorage│  │ (Theme +       │  │ Analytics Ctx │  │ Timer    │  │
│  │  session)    │  │  Global Pinyin)│  │ (Per-Chapter/ │  │ Hook     │  │
│  └──────┬───────┘  └───────┬────────┘  │  Per-Drill)   │  └────┬─────┘  │
│         │                  │           └───────┬───────┘       │        │
└─────────┼──────────────────┼───────────────────┼───────────────┼────────┘
          │                  │                   │               │
          ▼                  ▼                   ▼               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                          App Shell (RTL Layout)                         │
│  ┌───────────────────────────────────────────────────────────────────┐  │
│  │ Top Navbar: Brand · Nav Links · Pinyin Toggle · Theme · Logout    │  │
│  └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│  ┌────────────┐  ┌──────────────┐  ┌─────────────────┐  ┌────────────┐  │
│  │ Login Gate │─▶│ Welcome Home │─▶│ 15-Chapter Grid │─▶│ Analytics  │  │
│  └────────────┘  └──────────────┘  └────────┬────────┘  │ Dashboard  │  │
│                                             │           └────────────┘  │
│                                             ▼                           │
│                               ┌───────────────────────────┐             │
│                               │   Chapter Practice Hub    │             │
│                               │ (Practice / Exam Selector │             │
│                               │  + 9 Drills + Mixed Quiz) │             │
│                               └─────────────┬─────────────┘             │
│                                             │                           │
│                                             ▼                           │
│  ┌───────────────────────────────────────────────────────────────────┐  │
│  │                    Drill Session Orchestrator                     │  │
│  │  • Practice Mode: Instant Feedback  • Exam Mode: Free Navigation  │  │
│  │  • Final Review Screen  • Auto Google Sheets Webhook Sync         │  │
│  │                                                                   │  │
│  │  ┌───────────────────── 9 Modular Engines ─────────────────────┐  │  │
│  │  │ 1. Hanzi→Ar   2. Ar→Hanzi   3. Hanzi→Pinyin (5 options)     │  │  │
│  │  │ 4. 2-Col Match   5. 3-Col Match   6. Listening (zh-CN TTS)  │  │  │
│  │  │ 7. Sentence Trans (Bi-dir)  8. Sentence Builder  9. Cloze   │  │  │
│  │  └─────────────────────────────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼ (On Quiz Completion)
                     ┌────────────────────────────────┐
                     │ Google Sheets Sync Service     │
                     │ (no-cors JSON POST + fallback) │
                     └────────────────────────────────┘
```

### Data Model & State

- **Student Registry**: Static array of teacher-managed `{ id, name, code }` records (pre-seeded with 2 ready Arabic test accounts).
- **Chapter Curriculum Schema**: Strict TypeScript interfaces for `VocabItem` (`id`, `hanzi`, `pinyin`, `arabic`, optional `toneVariants`/`similarPinyin`), `SentenceItem` (`id`, `hanzi`, `pinyin`, `arabic`, `segments: string[]`, `targetBlank: string`, optional `blankOptions`), and `ChapterData` (`id: 1..15`, `titleAr`, `titleZh`, `vocab`, `sentences`).
- **Student Analytics Record** (stored per `studentId` in `localStorage`):
  - `totalSolved`, `correctCount`, `wrongCount`, `activeSeconds`
  - `studyDates: string[]` (ISO dates used to calculate consecutive Study Streak days)
  - `drillStats: Record<DrillType, { attempted: number; correct: number }>` (used to compute Weakest Drill Type)
  - `chapterStats: Record<number, { solved: number; correct: number; completedDrills: DrillType[] }>` (drives per-chapter accuracy and overall 15-chapter completion %)
  - `lastActive` timestamp and pending local sync log when `GOOGLE_SCRIPT_WEB_APP_URL` is empty.

### Interactive Component & State Mapping

- **Login Gate**: Trims and matches Name + Code against the allowed registry; surfaces clear inline Arabic validation errors on mismatch; unlocks the app shell and initializes that student's progress store on success.
- **Global Navbar Controls**:
  - Theme toggle switches `.dark` on `document.documentElement` and persists preference in `localStorage`.
  - Pinyin toggle updates React Context immediately, re-rendering Pinyin visibility across all mounted views without resetting drill progress.
- **9 Drill Engines**:
  - **Multiple-Choice Engines (1, 2, 3, 6, 7, 9)**: Stateless question renderers receiving `{ question, selectedAnswer, showFeedback, showPinyin, onSelect }`, enabling seamless reuse across both Practice Mode, Exam Mode, and Mixed Quiz sessions.
  - **2-Column & 3-Column Matching Engines (4, 5)**: Interactive column-by-column tap matching with clear selected state, paired lock-in animations, wrong-pair shake feedback, and partial/full score reporting back to the session orchestrator.
  - **Listening Engine (6)**: Wraps `window.speechSynthesis` with voice lookup prioritizing `zh-CN`, exposing `استماع عادي` (`rate: 1.0`) and `استماع بطيء` (`rate: 0.7`) controls, cancelling queued speech cleanly on unmount or replay.
  - **Sentence Builder Engine (8)**: Manages available shuffled `segments` pool and ordered answer slots; clicking a pool tile appends it to the sentence tray, clicking a placed tile returns it to the pool, with a clear reset control (`إعادة الترتيب`).
- **Google Sheets Sync**: Triggered automatically when a student completes any drill or exam; computes the exact 10-field JSON payload (`studentId`, `studentName`, `studentCode`, `totalSolved`, `correctCount`, `wrongCount`, `accuracyPercent`, `activeMinutes`, `overallProgressPercent`, `lastActive`) and transmits via `fetch(..., { method: 'POST', mode: 'no-cors' })` when configured, or records locally otherwise.
