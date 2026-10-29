# Aisle: grocery list design (v0.1)

Working name. A compact, Keep-style list built around three fixes:

1. **Find anything fast**, including items you checked off weeks ago.
2. **Stores are filters, not separate lists.** One master list. Costco, Aldi and Walmart views show what you can buy there.
3. **No settings screens.** Everything is edited in place: tap, hold, drag.

Clickable prototype: `prototype/aisle.html` (published as a private Claude artifact).

## Core ideas

| Thing | What it is |
|---|---|
| **Item** | Free text ("Eggs, 24 pack"). Belongs to one category. Either *on the list* or *checked off*. Never "used up": checked-off items wait in their category until you need them again. |
| **Category** | Your own groups, in your own order. Drag to reorder. Rename by tapping the name. |
| **Store** | A tag on an item. No tag = **Anywhere** (bananas). One or more tags = only those stores (paper towels → Costco). |
| **Store view** | Tap a store chip: that store's items **plus** anywhere items. Switch on "Hide items I can buy anywhere" to get a dedicated Costco-only list. |
| **Tabs (focus mode)** | One category per tab. Swipe sideways between them. |

## Screen

```
┌──────────────────────────────────────┐
│ [🔍 Add or find an item     ] [▭] [⋮]│  find-or-add bar · tabs/list switch · menu
│ (All 22) (●Costco 15) (●Aldi 15) (+) │  store chips with "to buy" counts
│  ○ Hide items I can buy anywhere  ✎  │  only when a store is selected
├──────────────────────────────────────┤
│ ⠿ Produce  5                   ⤢  ⌄ │  handle · name · count · focus · fold
│ ⠿ ☐ Bananas                          │
│ ⠿ ☐ Avocados          Costco  Aldi   │  store tags (blank = anywhere)
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
| Move an item in tab mode | Drag it onto another tab |
| Un-check by moving | Drag a checked-off item up into the list |
| Rename item, choose stores, delete | Tap its name → inline row of store chips |
| Reorder categories | Drag a category's ⠿ (everything folds to headers while you drag) |
| Rename / delete category | Tap its name |
| Add / rename / delete store | `+ Store` chip; pencil next to the store toggle |
| Undo a delete | "Undo" on the message at the bottom; no confirm dialogs |

## The find-or-add bar

One box does both jobs:

- **Empty + tapped:** shows *Recently checked off* as one-tap chips.
- **Typing:** instantly matches every item, on the list or checked off, with its category, stores and when you last checked it off. Tap → it's back on the list. Typos are forgiven ("chkn" finds "Chicken thighs").
- **New name + Enter:** adds it. The row shows where it will go ("to Produce"); category chips underneath let you pick another with one tap.
- **`@store` shortcut:** `paper towels @costco` tags it while adding.
- The keyboard stays open, so you can add five things in a row.

## Decisions I made (easy to change)

- New items default to **Anywhere**, even when added from a store view. A wrong "anywhere" stays visible; a wrong "Costco only" hides the item from your other stores. Exception: in Costco-only mode, new items get Costco.
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

1. "Manage tabs per item": did you mean choosing which **categories** get a tab, or something else, like pinning single items to a tab (a "This week" tab)?
2. Should adding an item while in a store view tag it with that store? (Currently: no, see above.)
3. Next step: go straight to the native Android app, or first make this an installable web app on your phone?
