# Aisle: grocery list design (v0.4)

Working name. A compact, Keep-style list built around three fixes:

1. **Find anything fast**, including items you checked off weeks ago.
2. **Stores are filters, not separate lists.** One master list. Costco, Aldi and Walmart views show what you can buy there.
3. **No settings screens.** Everything is edited in place: tap, hold, drag.

Clickable prototype: `prototype/aisle.html` (published as a private Claude artifact).

## Core ideas

| Thing | What it is |
|---|---|
| **Item** | Free text ("Eggs, 24 pack"). Belongs to one category. Either *on the list* or *checked off*. Never "used up": checked-off items wait in their category until you need them again. |
| **One-off item** | Something you'll buy once ("Birthday candles"). Removed when you check it off (with Undo), so it never clutters your history or search. |
| **Lookout item** | Something you're hunting for and might not find ("Calabrian chili paste"). Lives in a folded **👀 Keep an eye out** section at the bottom, not in the to-buy counts. Its stores mean *maybe at*: a store view only shows lookout items that might be there (or could be anywhere). Optional note ("only if under $8"). Check it off when you find it; inside a store view the app offers to remember that store. |
| **Category** | Your own groups, in your own order. Drag to reorder. Rename by tapping the name. |
| **Store** | A tag on an item. No tag = **Anywhere** (bananas). One or more tags = only those stores (paper towels → Costco). |
| **Store view** | Tap a store chip: that store's items **plus** anywhere items. Switch on "Hide items I can buy anywhere" to get a dedicated Costco-only list. |
| **Tabs (focus mode)** | Two ways to split the list into tabs: **Category** (one category per tab) or **Store** (Anywhere · Costco · Aldi · Walmart · One-offs). Swipe sideways between tabs. Store tabs show only items marked for that store, grouped by category. |

## Screen

```
┌──────────────────────────────────────┐
│ [🔍 Add or find an item     ] [▭] [⋮]│  find-or-add bar · tabs/list switch · menu
│ (All 22) (●Costco 15) (●Aldi 15) (+) │  store chips with "to buy" counts
│  ○ Hide items I can buy anywhere  ✎  │  only when a store is selected
├──────────────────────────────────────┤
│ ⠿ Produce  5                   ⤢  ⌄ │  handle · name · count · focus · fold
│ ⠿ ☐ Bananas                        ⋮ │  ⋮ = this item's options
│ ⠿ ☐ Avocados        Costco  Aldi   ⋮ │  store tags (blank = anywhere)
│   ┆ New item_                        │  writing line: type, Enter, type…
│     ✓ 4 checked off ›                │  tucked away, one tap to open
│ ⠿ Dairy & eggs  3              ⤢  ⌄ │
│   …                                  │
│ [ + New category ]                   │
└──────────────────────────────────────┘
```

## Gestures (the whole "settings" surface)

| To do this | Do this |
|---|---|
| Check off / put back | Tap the box |
| Move an item (any category) | Hold the item, or drag its ⠿ handle |
| Move an item to another category (tabs) | Drag it onto a category tab |
| Change where it's bought (tabs) | Drag it onto a store tab: Anywhere, a store (that store only), or One-offs |
| Un-check by moving | Drag a checked-off item up into the list |
| Add items | Tap **+ Add item** under a category and type. Enter = next line |
| Rename an item | Tap its name and type, like editing a note. Enter = new line below it |
| Erase an item while writing | Clear its text and press Backspace once more (Undo available) |
| Choose stores, make it a one-off or a lookout item, move it, delete | The item's **⋮** button |
| Start or stop keeping an eye out | Drag an item into or out of **👀 Keep an eye out** |
| Reorder categories | Drag a category's ⠿ (everything folds to headers while you drag) |
| Rename / delete category | Tap its name |
| Add / rename / delete store | `+ Store` chip; pencil next to the store toggle |
| Undo a delete | "Undo" on the message at the bottom; no confirm dialogs |

## Writing a list

Adding should feel like writing on paper:

- Every category ends with a faint **+ Add item** line. Tap it and type.
- **Enter** saves the line and opens the next one right below it, so the keyboard stays up. **Enter on an empty line** finishes.
- **Backspace on an empty line** jumps back up to the previous item to fix it.
- Writing a name that's already on file (say "Garlic", checked off last week) puts that item back on the list instead of making a duplicate.
- **Paste a list** (from Keep, a note, a text) and every line becomes an item. Bullets and checkboxes are cleaned up. Ticked lines (☑, [x]) come in as checked off.
- New lines follow the same store rule as everything else: Anywhere on the main list, the store's tag inside a store view or store tab.

## The find-or-add bar

One box does both jobs:

- **Empty + tapped:** shows *Recently checked off* as one-tap chips.
- **Typing:** instantly matches every item, on the list or checked off, with its category, stores and when you last checked it off. Tap → it's back on the list. Typos are forgiven ("chkn" finds "Chicken thighs").
- **New name + Enter:** adds it. The row shows where it will go ("to Produce"); category chips underneath let you pick another with one tap.
- **Add as a one-off:** a second row under "Add …", one tap.
- **Keep an eye out for it:** a third row, one tap.
- **Shortcuts while typing:** `@costco` marks a store, `@anywhere` clears stores, `@once` makes a one-off, `@lookout` keeps an eye out.
- The keyboard stays open, so you can add five things in a row. Pasting a multi-line list here works too.

## Decisions I made (easy to change)

- **Where new items go:** on the main list (All), new items are **Anywhere**. Inside a store (the Costco chip, or the Costco store tab), they're marked **Costco**. The add row always shows this before you add, and you can change it later from the item's ⋮ menu.
- Checked-off items sort most recent first.
- Empty categories are hidden inside store views (less clutter), shown in All.
- In the prototype the bar is at the top so the phone keyboard can't cover it. The Android app can dock it at the bottom for thumbs.

## Not in v1, on purpose

Prices, quantity fields (type "x2" in the name), sharing between people, multiple master lists, auto-sorting into categories, barcode scanning.

## Saving (being replaced: see SPEC.md, Hosting)

- **Prototype:** saved in the browser, and privately to your Claude account when opened on claude.ai (follows you between phone and computer). Menu → *Copy backup* gives you a text copy.
- **Android app:** an on-device database. No account, no internet, nothing leaves the phone. Keeps *Copy backup / Restore* so you can move phones.

## Path to the Android app

1. **Now: nail the UX** with this prototype. Use it for a real shopping trip or two.
2. **Native Android app**: built with Google's standard Android toolkit (Kotlin + Jetpack Compose) and the phone's built-in database. Installed directly from a file (no Play Store). Same screens and gestures, plus haptics and smoother dragging.
3. **Later**: home-screen widget, sharing with a household, whatever the trips teach us.

## Open questions

1. Next step: go straight to the native Android app, or first make this an installable web app on your phone?

Settled: lookout items in a folded section, stores meaning "maybe at" (v0.4). Tabs can be split by category or by store/one-off (v0.2); items added in a store view are marked for that store (v0.2); adding works like writing, and item options live behind ⋮ (v0.3). Other input styles can be trialed later.
