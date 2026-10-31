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
| **Category** | Your own groups, in your own order. Drag to reorder. Rename by tapping the name. |
| **Store** | A tag on an item. No tag = **Anywhere** (bananas). One or more tags = only those stores (paper towels → Costco). |
| **Store view** | Tap a store chip: that store's items **plus** anywhere items. Switch on "Hide items I can buy anywhere" to get a dedicated Costco-only list. |
| **Category tabs** | Always visible under the store chips: **All · Produce · Dairy · … · +**. *All* is the whole list; a category tab shows just that category. Tap or swipe sideways to switch. Inside a store view, only categories with something for that store get a tab. |

## Screen

```
┌──────────────────────────────────────┐
│ [🔍 Add or find an item         ] [⋮]│  find-or-add bar · menu
│ (All 22) (●Costco 15) (●Aldi 15) (+) │  store chips with "to buy" counts
│  ○ Hide items I can buy anywhere  ✎  │  only when a store is selected
│  All  Produce 5  Dairy 3  Meat 2  …  │  category tabs (swipe or tap)
├──────────────────────────────────────┤
│ ⠿ Produce  5                      ⌄ │  handle · name · count · fold
│ ⠿ ☐ Bananas                        ⋮ │  ⋮ = this item's options
│ ⠿ ☐ Avocados        Costco  Aldi   ⋮ │  store tags (blank = anywhere)
│   ┆ New item_                        │  writing line: type, Enter, type…
│     ✓ 4 checked off ›                │  tucked away, one tap to open
│ ⠿ Dairy & eggs  3                 ⌄ │
│   …                                  │
│ [ + New category ]                   │
└──────────────────────────────────────┘
```

## Gestures (the whole "settings" surface)

| To do this | Do this |
|---|---|
| Check off / put back | Tap the box |
| Move an item (any category) | Hold the item, or drag its ⠿ handle |
| Move an item to another category | Drag it onto that category's tab (or into the category) |
| Mark where it's bought | Drag it onto a store chip (makes it that store only; Undo available) |
| Un-check by moving | Drag a checked-off item up into the list |
| Add items | Tap **+ Add item** under a category and type. Enter = next line |
| Rename an item | Tap its name and type, like editing a note. Enter = new line below it |
| Erase an item while writing | Clear its text and press Backspace once more (Undo available) |
| Choose stores, make it a one-off, move it, delete | The item's **⋮** button |
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
- New lines follow the same store rule as everything else: Anywhere on the main list, the store's tag inside a store view.

## The find-or-add bar

One box does both jobs:

- **Empty + tapped:** shows *Recently checked off* as one-tap chips.
- **Typing:** instantly matches every item, on the list or checked off, with its category, stores and when you last checked it off. Tap → it's back on the list. Typos are forgiven ("chkn" finds "Chicken thighs").
- **New name + Enter:** adds it. The row shows where it will go ("to Produce"); category chips underneath let you pick another with one tap.
- **Add as a one-off:** a second row under "Add …", one tap.
- **Shortcuts while typing:** `@costco` marks a store, `@anywhere` clears stores, `@once` makes a one-off.
- The keyboard stays open, so you can add five things in a row. Pasting a multi-line list here works too.

## Decisions I made (easy to change)

- **Where new items go:** on the main list (All), new items are **Anywhere**. Inside a store (the Costco chip selected), they're marked **Costco**. The add row always shows this before you add, and you can change it later from the item's ⋮ menu.
- Checked-off items sort most recent first.
- Empty categories are hidden inside store views (less clutter), shown in All.
- In the prototype the bar is at the top so the phone keyboard can't cover it. The Android app can dock it at the bottom for thumbs.

## Not in v1, on purpose

Prices, quantity fields (type "x2" in the name), sharing between people, multiple master lists, auto-sorting into categories, barcode scanning.

## Saving

- **Prototype:** saved in the browser, and privately to your Claude account when opened on claude.ai (follows you between phone and computer). Menu → *Copy backup* gives you a text copy.
- **Android app:** an on-device database. No account, no internet, nothing leaves the phone. Keeps *Copy backup / Restore* so you can move phones.

## Path to the Android app

1. **Now: nail the UX** with this prototype. Use it for a real shopping trip or two.
2. **Native Android app**: built with Google's standard Android toolkit (Kotlin + Jetpack Compose) and the phone's built-in database. Installed directly from a file (no Play Store). Same screens and gestures, plus haptics and smoother dragging.
3. **Later**: home-screen widget, sharing with a household, whatever the trips teach us.

## Open questions

1. Next step: go straight to the native Android app, or first make this an installable web app on your phone?

Settled: tabs are categories only and always visible, stores stay as chips (v0.4); items added in a store view are marked for that store (v0.2); adding works like writing, and item options live behind ⋮ (v0.3). Other input styles can be trialed later.
