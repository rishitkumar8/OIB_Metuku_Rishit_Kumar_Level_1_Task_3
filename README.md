# 🌡️ Premium Temperature Converter

> **Oasis Infobyte Web Development Internship · Level 1 · Task 3**  
> *An elegant, glassmorphism-styled temperature converter with real-time validation, conversion history, and toast notifications.*

---

## Youtube Video Link
[Youtube Video of Task 3 - Temperature Converter](https://youtu.be/S3P9Ian9I7w)

## 📌 Task Overview

This project is a **temperature converter web application** built as part of the Oasis Infobyte Web Development Internship curriculum. The core requirements were to build a tool where a user can:

1. **Input** a temperature value
2. **Select** the source and target units (Celsius / Fahrenheit)
3. **Press a button** to trigger the conversion
4. **View the result** in a clearly labelled display area
5. *(Extra Challenge)* Support **Kelvin** as a third conversion unit

All four required elements are implemented — plus several premium UX enhancements that go well beyond the minimum spec.

---
## ✨ Features

| Feature | Description |
|---|---|
| 🔢 **Temperature Input** | Number field with validation — rejects non-numbers and values below absolute zero |
| 🔽 **Dual Dropdowns** | Separate "From" and "To" selectors for maximum conversion flexibility |
| 🔁 **Swap Button** | One-click unit swap with animated rotate effect |
| ✖️ **Clear Button** | Appears inside the input when a value is typed; clears and refocuses |
| 🔴 **Inline Error Messages** | Contextual error text that animates in/out beneath the input |
| ⚡ **Convert Button** | Triggers conversion with a 400ms loading spinner for a premium feel |
| 📐 **Formula Display** | Shows the exact mathematical formula used for the conversion |
| 📋 **Copy to Clipboard** | Copy the result value with one click |
| 🕐 **Conversion History** | Keeps the last 5 conversions with timestamps |
| 🔔 **Toast Notifications** | Slide-in pop-up confirmations for actions (convert, copy, swap) |
| 🔢 **Animated Counter** | Result number animates smoothly from old value to new (easeOutQuart) |
| 🌈 **Glassmorphism UI** | Frosted glass card with animated blob background |
| 📱 **Fully Responsive** | Layout adapts cleanly to mobile screens |

---

## 🗂️ Application Structure

```
Temperature Converter
│
├── 🌈 Background Blobs      — Two animated gradient blobs (purple-pink + green-blue)
│
└── 🪟 Glass Card
    ├── 📰 Header             — Title · Subtitle
    ├── 🔢 Input Group        — Number field · Clear (×) button · Error message
    ├── ⚙️ Controls Group     — From dropdown · Swap button · To dropdown
    ├── ⚡ Convert Button      — With spinner animation
    ├── 📊 Result Section     — Result value + unit · Formula · Copy button
    └── 🕐 History Section    — Last 5 conversions with timestamps
```

---

## 🧮 Conversion Logic

All conversions route through **Celsius as an intermediate unit**:

```
Input → Convert to °C → Convert °C to Target
```

### Supported Conversions (all 6 directions)

| From | To | Formula |
|---|---|---|
| **°C** | **°F** | `(°C × 9/5) + 32` |
| **°F** | **°C** | `(°F - 32) × 5/9` |
| **°C** | **K** | `°C + 273.15` |
| **K** | **°C** | `K - 273.15` |
| **°F** | **K** | `(°F - 32) × 5/9 + 273.15` |
| **K** | **°F** | `(K - 273.15) × 9/5 + 32` |

Each conversion also displays the **human-readable formula** with the actual input values substituted in, so the user can see exactly how the result was calculated.

---

## ✅ Input Validation Rules

The converter validates on every keystroke and again on convert, catching three error cases:

| Condition | Error Message |
|---|---|
| Field is empty on convert | *"Please enter a value to convert."* |
| Input is not a valid number | *"Please enter a valid number."* |
| Value is below absolute zero for the selected unit | *"Value cannot be below absolute zero (−273.15°C / −459.67°F / 0K)."* |
| Same unit selected for From and To | *"Please select different units to convert."* |

Errors are displayed with a smooth slide-down animation and turn the input border red. The Convert button is automatically **disabled** while an error is present.

---

### Visual Effects
- **Glassmorphism** — `backdrop-filter: blur(20px)` + semi-transparent white background
- **Animated Blobs** — Two large blurred gradient circles in the background that float continuously via `@keyframes float`
- **Focus Ring** — Custom `box-shadow` focus indicator using `--focus-ring` variable

---

## ⚙️ JavaScript Functionality Deep Dive

### Event Listeners
| Event | Element | Action |
|---|---|---|
| `input` | Temperature field | Toggles clear button visibility, validates |
| `keypress` (Enter) | Temperature field | Triggers `validateAndConvert()` |
| `click` | Clear button | Empties field, hides clear button, removes errors |
| `click` | Swap button | Swaps From/To selects, re-validates, shows toast |
| `click` | Convert button | Validates + runs conversion with loading state |
| `click` | Copy button | Copies result to clipboard via `navigator.clipboard` |
| `change` | Both selects | Re-validates current input against new unit |

### Key Functions

#### `validateInput()`
Runs on every change. Checks: empty field, `isNaN()`, below absolute zero using the `ABS_ZERO` constants object, and same-unit selection. Calls `showError()` or `hideError()` accordingly and enables/disables the Convert button.

#### `validateAndConvert()`
Entry point for conversion. Runs final validation, then simulates a 400ms loading state (button disabled, text hidden, spinner shown) before calling `performConversion()`.

#### `convertTemperature(value, from, to)`
Pure conversion function. Converts input to °C, then from °C to the target unit. Returns both the numeric result and the human-readable formula string.

#### `animateNumber(start, end, element)`
Uses `requestAnimationFrame` with an `easeOutQuart` easing function `(1 - (1-t)^4)` to smoothly animate the result number from the previous value to the new one over 600ms.

#### `addToHistory(val, from, res, to)`
Prepends a new entry to the `conversionHistory` array, trims it to `MAX_HISTORY` (5), then calls `renderHistory()` to update the DOM.

#### `showToast(message, type)`
Dynamically creates a toast `<div>`, appends it to the toast container (which uses `slideIn` animation), then removes it after 3 seconds with a `fadeOut` animation.

---

## 🌡️ Absolute Zero Reference

The app uses a lookup object to validate inputs against physics:

```javascript
const ABS_ZERO = {
  'C': -273.15,   // −273.15 °C
  'F': -459.67,   // −459.67 °F
  'K': 0          //   0 K
};
```

Any value at or below the absolute zero for the selected input unit is rejected with an informative error.

---

## 🛠️ Technologies Used

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![Google Fonts](https://img.shields.io/badge/Google_Fonts-4285F4?style=flat&logo=google&logoColor=white)

- **HTML5** — Semantic structure, accessible labels, ARIA attributes
- **CSS3** — Custom properties, `backdrop-filter`, `@keyframes`, transitions, media queries
- **Vanilla JavaScript** — DOM manipulation, `requestAnimationFrame`, `navigator.clipboard`, Closures
- **Google Fonts** — `Inter` typeface
- **No frameworks, no libraries** — 100% vanilla implementation

---

## 📂 File Info

```
Metuku Rishit Kumar_Task3.html    ← Single self-contained file (HTML + CSS + JS)
```

> All styles are embedded in `<style>` tags and all scripts in `<script>` tags.  
> No external dependencies need to be installed — just open in a browser.

---

## 🚀 How to Run

1. Download or clone this repository
2. Open `Metuku Rishit Kumar_Task3.html` in any browser (Chrome, Firefox, Edge, Safari)
3. Type a temperature value, choose units, and click **Convert**
4. No installation, no build step, no server required

---

## 🧪 Quick Test Cases

| Input | From | To | Expected Output |
|---|---|---|---|
| `100` | °C | °F | `212 °F` |
| `32` | °F | °C | `0 °C` |
| `0` | °C | K | `273.15 K` |
| `300` | K | °C | `26.85 °C` |
| `-459.67` | °F | °C | ❌ Error (absolute zero) |
| `abc` | °C | °F | ❌ Error (invalid number) |

---

## 👨‍💻 Author

**Metuku Rishit Kumar**  
Oasis Infobyte Web Development Internship  
*Level 1 · Task 3 — Temperature Converter*

---

*Precise calculations. Beautiful interface. Zero friction. 🌡️*
