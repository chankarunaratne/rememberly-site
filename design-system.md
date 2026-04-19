# Rememberly Design System: Color Architecture

This document outlines the three-layer architecture for the Rememberly color design system. This tiered approach is an industry-standard best practice (similar to Material Design and modern Tailwind ecosystems) that ensures scalability, easy themeability (like adding dark mode), and high reusability without changing component CSS.

---

## 🏗️ Architecture Overview

The color system is broken down into three distinct layers, each serving a specific structural purpose:

1. **Layer 1: Primitive Tokens** (The "What" - literal hex codes)
2. **Layer 2: Semantic Tokens** (The "Intent" - contextual scales)
3. **Layer 3: Mapped / Application Tokens** (The "Where" - component-specific usage)

---

## 🎨 Layer 1: Primitive Tokens

**What they are:** 
Primitives are the absolute foundational building blocks of the design system. They map a literal hex code to a literal name. 

**Educational Note:** 
We *never* add meaning or intent to primitive names (e.g., we never name a color "brand" or "background" here). We name them based on their visual hue (like `Orange`, `Stone`, `Red`). This is strictly a palette. If the brand changes from Orange to Purple in two years, the `Orange` primitive remains orange, and we simply introduce a `Purple` primitive. 

For the "Sepia/Reading Vibe", we use a warm grey/brown scale called **Stone** instead of a cold, neutral grey.

```css
/* =========================================
   LAYER 1: PRIMITIVES
   Naming convention: --color-[hue]-[shade]
========================================= */

/* WHITE & BLACK */
--color-white: #FFFFFF;
--color-black: #000000;

/* STONE (Our Sepia/Warm Grey base) */
--color-stone-50:  #FAF6F3;
--color-stone-100: #F8F1EB; /* Current Background */
--color-stone-200: #EBE1D8;
--color-stone-300: #E0D5CC; /* Current Muted Borders */
--color-stone-400: #A89D95;
--color-stone-500: #8D8078;
--color-stone-600: #746861; /* Current Secondary Text */
--color-stone-700: #574F49;
--color-stone-800: #423B36;
--color-stone-900: #332A24; /* Current Primary Text */

/* ORANGE (Our Claude-like accent) */
--color-orange-50:  #FFF5F1;
--color-orange-100: #FCE8E1;
--color-orange-200: #F8CBB8;
--color-orange-300: #F5AA8B;
--color-orange-400: #EE8A62;
--color-orange-500: #DF6840; /* Current CTA Accent */
--color-orange-600: #C55631; /* Current CTA Hover */
--color-orange-700: #AA4525;
--color-orange-800: #8D3A20;
--color-orange-900: #753420;

/* RED (For future error states) */
--color-red-100: #FDE8E8;
--color-red-500: #E02424; 

/* GREEN (For future success states) */
--color-green-100: #DEF7EC;
--color-green-500: #0E9F6E;

/* BLUE (For future info states) */
--color-blue-100: #E1EFFE;
--color-blue-500: #3F83F8;

/* YELLOW (For future warning states) */
--color-yellow-100: #FDF6B2;
--color-yellow-500: #E3A008;
```

---

## 🧠 Layer 2: Semantic Tokens

**What they are:** 
Semantic tokens map our Primitives to abstract, meaning-based scales. This is the "bridge" layer.

**Educational Note:** 
This is where we assign *intent*. We map our `Orange` to `Primary`, our `Stone` to `Neutral`, our `Red` to `Danger`, etc. 
If your "Brand" color ever changes from Orange to Blue, this is the **ONLY** place in your entire codebase you have to make a change. You just update `--primary-500: var(--color-blue-500);` and the whole site updates instantly.

```css
/* =========================================
   LAYER 2: SEMANTICS
   Naming convention: --[intent]-[shade]
========================================= */

/* NEUTRAL SCALE (Derived from Stone) */
--neutral-50: var(--color-stone-50);
--neutral-100: var(--color-stone-100);
--neutral-200: var(--color-stone-200);
--neutral-300: var(--color-stone-300);
--neutral-400: var(--color-stone-400);
--neutral-600: var(--color-stone-600);
--neutral-900: var(--color-stone-900);

/* PRIMARY SCALE (Derived from Orange) */
--primary-100: var(--color-orange-100);
--primary-500: var(--color-orange-500);
--primary-600: var(--color-orange-600);
--primary-700: var(--color-orange-700);

/* SYSTEM SCALES (Success, Danger, Info, Warning) */
--danger-100: var(--color-red-100);
--danger-500: var(--color-red-500);

--success-100: var(--color-green-100);
--success-500: var(--color-green-500);

--info-100: var(--color-blue-100);
--info-500: var(--color-blue-500);

--warning-100: var(--color-yellow-100);
--warning-500: var(--color-yellow-500);
```

---

## 🧩 Layer 3: Mapped / Application Tokens

**What they are:**
Also known as "Component" or "UI" tokens. These dictate exactly *where* and *how* a color is applied. They reference the Semantic tokens.

**Educational Note:**
This layer makes writing components incredibly easy and predictable. When styling a button, you don't say `background-color: var(--primary-500)`. Instead, you say `background-color: var(--action-primary-default)`. 

Why? Because if a user toggles Dark Mode, `var(--action-primary-default)` can be re-mapped inside a media query or `.dark` class to accommodate the new theme, while `--primary-500` strictly remains the middle orange.

```css
/* =========================================
   LAYER 3: MAPPED / UI TOKENS
   Naming convention: --[category]-[property]-[intent/state]
========================================= */

/* BACKGROUNDS / SURFACES */
/* The main page body background */
--bg-canvas: var(--neutral-100); 
/* Things resting on the canvas (cards, modals) */
--bg-surface: var(--color-white); 
/* A slightly darker surface for contrast */
--bg-surface-alt: var(--neutral-50); 
/* Dark overlay behind modals to dim the app */
--bg-overlay: rgba(51, 42, 36, 0.4); 

/* FORMS & INPUTS */
/* Background color for form fields */
--bg-input: var(--neutral-50);
/* Default input border */
--border-input: var(--neutral-300);
/* Active/Focus ring for inputs */
--border-input-focus: var(--primary-500);

/* TEXT & TYPOGRAPHY */
/* High contrast body text / Headings */
--text-primary: var(--neutral-900);
/* Muted text / Subheadings */
--text-secondary: var(--neutral-600);
/* Placeholder text inside inputs */
--text-placeholder: var(--neutral-400);
/* Text intended to sit on top of the primary action color (e.g. inner button text) */
--text-on-primary: var(--color-white);
/* Error text */
--text-danger: var(--danger-500);

/* BORDERS & DIVIDERS */
/* Default gentle border */
--border-subtle: var(--neutral-300);
/* Danger outline */
--border-danger: var(--danger-500);

/* ACTIONS / BUTTONS */
/* Primary Action */
--action-primary-default: var(--primary-500);
--action-primary-hover: var(--primary-600);
--action-primary-pressed: var(--primary-700);

/* Secondary Action */
--action-secondary-default: var(--neutral-200);
--action-secondary-hover: var(--neutral-300);

/* Disabled State */
--action-disabled-bg: var(--neutral-200);
--action-disabled-text: var(--neutral-400);
```

### Summary of How the Engine Works:
If you have an `index.html` file with a button, the CSS resolution path looks like this:

`button { color: var(--text-on-primary); background-color: var(--action-primary-default); }`
* The Browser sees `--action-primary-default`
* It goes to Layer 3 and sees it maps to `--primary-500`
* It goes to Layer 2 and sees it maps to `--color-orange-500`
* It goes to Layer 1 and renders `#DF6840`

This separation of concerns means your CSS logic and your brand colors are completely decoupled!

---

## 🚀 New Updates (App-Ready Architecture)

We recently expanded this design system from a simple web landing page structure into a complete, scalable architecture ready for mobile apps (iOS/SwiftUI) and complex web applications. 

### What was added:
1. **Full 10-Step Scales:** Filled out the `50` to `900` math for both Stone (Neutral) and Orange (Primary) to ensure developers never have to guess literal hex codes for hover states or subtle backgrounds.
2. **System Feedback Colors:** Introduced Red (Danger), Green (Success), Blue (Info), and Yellow (Warning) for comprehensive app status messages and badges.
3. **Interactive States:** Expanded action tokens beyond `hover` to include `pressed` (vital for mobile touch feedback) and `disabled`.
4. **Secondary Hierarchy:** Introduced `action-secondary` tokens for soft, non-primary buttons like "Cancel" or "Skip".
5. **App Surfaces & Forms:** Added `--bg-overlay` utilizing a dark transparent Stone for modal scrims, and form-specific tokens (`--bg-input`, `--border-input`, `--text-placeholder`) for clean data entry interfaces.
