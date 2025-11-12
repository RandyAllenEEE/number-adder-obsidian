# Obsidian Number Adder Plugin

This is a plugin designed for [Obsidian](https://obsidian.md) that automatically adds hierarchical numbering to Markdown headings and supports numbering for math formula blocks (`$$...$$`).

The core code and concept are derived from the [number-headings](https://github.com/onlyafly/number-headings-obsidian) plugin. Thank you!

This plugin is powerful and highly configurable. All settings apply individually to each file and are stored in the file's Front Matter.

## Core Features

* **Multi-level Heading Numbering**:
    * Automatically adds hierarchical numbering to H1 through H6 headings (e.g., `1.`, `1.1.`, `1.1.1.`).
    * Provides fine-grained control via a unified "Numbering Control" panel.
* **Flexible Style Customization**:
    * **Style by Level**: Allows setting the numbering style *individually* for each of the 1–6 heading levels.
    * **Supported Styles**:
        * `1`: Arabic numerals (1, 2, 3...)
        * `a`: Lowercase letters (a, b, c...)
        * `A`: Uppercase letters (A, B, C...)
        * `一`: Chinese numerals (一, 二, 三...)
        * `①`: Circled numbers (①, ②, ③...)
    * **Separator by Level**: Customize the separator between levels (e.g., between H1 and H2), such as `.`, `-`, `:`, etc.
    * **Start Value by Level**: Set a starting number for *each* heading level, including starting from `0` (e.g., `零` or `⓪`).
* **Advanced Formula Numbering**:
    * Automatically adds `\tag{...}` numbering to `$$...$$` math blocks.
    * Supports two modes:
        1.  **Continuous**: Numbers increment throughout the document as `(1)`, `(2)`, `(3)`...
        2.  **Heading-based**: Generates numbers based on the previous heading’s number, e.g., `(1.1-1)`, `(1.1-2)`, `(2.1-1)`...
* **Unified Control Panel**:
    * Opens a modal via the `Number Control` command, providing access to all features.
    * **Manual Numbering**: Immediately number headings, formulas, or both, or remove all numbering.
    * **Auto Numbering Settings**: Configure all styles, ranges, start values, and whether to auto-number headings or formulas.
* **Front Matter-based Configuration**:
    * All settings apply to *individual files* and are saved in a compact format under the Front Matter keys `number headings` and `number formulas`.
    * This allows each document to have its own independent numbering scheme without changing global settings.
* **Automatic Table of Contents (TOC) Generation**:
    * (Retained feature) Supports adding an anchor (e.g., `^toc`) in headings, and the plugin will automatically generate a table of contents linking to all headings under that heading.

## How to Use

1. Open the Command Palette (Cmd/Ctrl + P).
2. Search for and run the **"Number Control"** command.
3. **In the "Manual Numbering" tab**:
    * Click **"Number Headings"** or **"Number Formulas"** to apply numbering immediately.
    * Click **"Remove All Numbering"** to clear all numbering in the current document.
4. **In the "Auto Numbering Settings" tab**:
    * Turn the **"Auto Number Headings"** or **"Auto Number Formulas"** switches on or off as needed.
    * Select your **"Formula Numbering Mode"** (Continuous or Heading-based).
    * Set the **"Numbering Range"** you wish to apply (e.g., from H1 to H6).
    * **Configure Styles**: Select the "Type", "Separator", and "Start Value" for each of the 1–6 heading levels.
    * Click **"Save Auto Numbering Settings and Apply Once"**.
5. Your settings will be saved to the file's Front Matter and applied once immediately. If auto-numbering is enabled, the plugin will automatically update based on your set refresh interval.

## Advanced Front Matter Configuration

All settings are stored in a compact format under the `number headings` and `number formulas` keys.

### `number headings`

Format: `[auto], [range], [styles], [separators], [start-values], [options]`

* **`auto`**: (Optional) If present, automatically numbers headings.
* **`range`**: Level range, e.g., `1-6`.
* **`styles`**: A 6-character string corresponding to the style for H1–H6. e.g., `1aA一①1`.
* **`separators`**: A 5-character string for separators between H1-H2, H2-H3...H5-H6. e.g., `-:.—`.
* **`start-values`**: A 6-character string for the starting value of H1–H6. e.g., `011111` (H1 starts from 0, others from 1).
* **`options`**: (Optional)
    * `contents ^toc`: Sets a table of contents anchor.
    * `skip ^skip`: Skips headings containing the `^skip` anchor.

**Example:**
`number headings: auto, 1-6, 1aA一①1, -:.—, 011111, contents ^toc`

### `number formulas`

Format: `[auto], [mode]`

* **`auto`**: (Optional) If present, automatically numbers formulas.
* **`mode`**: `continuous` or `heading-based`.

**Example:**
`number formulas: auto, heading-based`

---

## Changelog

### 2.0.0 

* **Major Change:** Introduced `headingStyles`, `headingSeparators`, and `headingStartValues` arrays, allowing *individual* configuration of style, separator, and start value for each of the 1–6 heading levels.
* **New Feature:** Added support for **Chinese numerals** (`一, 二, 三`) and **circled numerals** (`①, ②, ③`).
* **New Feature:** Added **"Heading-based"** (`heading-based`) mode for formula numbering (e.g., `1.1-1`, `1.1-2`).
* **New Feature:** Introduced a unified **"Numbering Control"** modal that integrates all manual and auto settings.
* **Major Change (Removed):** Removed support for Roman numerals (`I`, `i`) to simplify maintenance and core logic.
* **Major Change (Removed):** Removed the old, single `start-at` setting. More powerful per-level control is now provided by the `headingStartValues` array.
* **Major Change (Removed):** Removed all old Front Matter keys (e.g., `styleLevel1`, `styleLevelOther`, `separator`, `auto`, etc.). All settings are now uniformly stored in the compact strings under the `number headings` and `number formulas` keys.

---

### Legacy Changelog (1.12.0 and earlier)

* 1.12.0: (Removed) Added support for Roman numerals.
* 1.11.0: (Removed) Added 'start-at' setting.
* 1.10.1: Fixed a loop error caused by separators.
* 1.8.0: Added automatic TOC rendering and 'first-level' setting.
* 1.7.0: (Removed) Added support for custom separators (now upgraded to per-level definition).
* 1.6.0: (Removed) Simplified Front Matter settings to a single key (now updated to new compact format).
