# Aisle: spec for v0.4 (suggestions, lookout items, outside input)

Builds on [DESIGN.md](DESIGN.md) (v0.3). This covers what's new; everything else stays as designed.

## Goals

1. **The list builds itself.** No catalog setup. Anything you type is remembered.
2. **Starting a list takes seconds.** The app drafts it from your habits; you accept or reject.
3. **Things can come in from anywhere**: pasted text, phone dictation, or another AI.
4. **Odd items have a home**: one-time buys and "keep an eye out for this" items.

## Not doing (yet)

- **Our own voice input.** The phone's keyboard microphone already types into the add bar. Built-in voice adds cost and little value.
- **A chat screen.** It takes too much room. Pasting covers the useful part.
- **Pantry tracking** (what's in stock at home).
- **Browsing by grid.** Might test later as a separate mode.

---

## 1. Suggested draft list

**What you see:** when you open the app and some items are probably due, a **Suggested** section appears at the top:

```
┌──────────────────────────────────────┐
│ Suggested for this trip      Add all │
│  + Milk           usually every 7 d ✕│
│  + Coffee beans   usually every 3 wk ✕│
│  + Paper towels   Costco · every 6 wk ✕│
└──────────────────────────────────────┘
```

- Tap **+** to add one, **Add all** to add the rest, **✕** to dismiss.
- Dismissing lasts until that item is due again. It is not a permanent no.
- If you dismiss an item 3 times in a row, it stops being suggested until you buy it again.
- Nothing is ever added without your tap.

**How it learns (no setup):**
- Each check-off is recorded with its date.
- After **3 purchases**, the app knows the item's typical gap (for example, about 7 days).
- An item is suggested once ~90% of its typical gap has passed since you last bought it.
- Items you buy at irregular times never qualify, so they never get suggested.

**Later (v0.5):** learn your usual shopping day. Get an optional reminder the evening before ("Draft list ready: 6 items").

**Done when:** after a few weeks of real use, at least half of the suggested items get accepted.

## 2. One-off items

You already can mark something as a one-off (DESIGN.md). It disappears when checked off. That stays, but **you shouldn't have to remember to use it**:

- Items you buy once and then don't touch for **90 days** quietly drop out of suggestions and the "recently checked off" chips.
- They still turn up if you search for them by name. Nothing is deleted.

So marking a one-off is a shortcut, not a chore. Sorting old, unused items to the bottom handles the clutter.

## 3. Lookout items ("keep an eye out")

For things you might not find anywhere, or only by luck: a specific hot sauce, seasonal items, a replacement part.

**What it is:** a third state alongside *on the list* and *checked off*: **On the lookout**.

| | On the list | On the lookout |
|---|---|---|
| Meaning | Buy it this trip | Grab it if you see it |
| Store tags | Where it's sold | **Maybe at**: where it might turn up (can be blank = anywhere) |
| Counted in "to buy" numbers | Yes | No |
| Shown | In its category | A folded **👀 Keep an eye out (3)** section at the bottom of each store view where it might be |
| Optional note | — | A short condition, e.g. "only if under $15" |

**When you find it:** check it off. The app asks once: *"Found at Costco? Remember that."* Yes → Costco becomes a store tag and the item turns into a normal item. You can also just check it off without answering.

**How to make one:** item's **⋮** → *Keep an eye out*, or type `@lookout` in the add bar.

## 4. Getting items in from elsewhere

In order of effort:

1. **Phone dictation (already works).** Tap the keyboard mic in the add bar. Extend the paste/typing rules so that "milk, eggs and dish soap" becomes 3 items.
2. **Paste anything (already designed).** Recipe ingredient lists, a text from your partner, a Keep note. Every line or comma-separated entry becomes an item, matched to existing items so there are no duplicates.
3. **Connect any AI (MCP).** "MCP" is a standard plug that lets AI assistants (Claude, ChatGPT and others) use an app. Tell any assistant "add the ingredients for this recipe to my grocery list" and it does.
   - What the AI can do: add items (with store and lookout options), read the current list, and check things off.
   - Same rules as typing: names already on file come back instead of being duplicated.
   - ⚠️ **Tradeoff:** an AI can only reach your list if the list lives online somewhere. DESIGN.md currently says "nothing leaves the phone." See open question 1.

## Changes to the data

Each item gains:
- **Purchase dates**: every check-off date (powers suggestions and the 90-day fade).
- **State**: on the list / checked off / **on the lookout**.
- **Lookout note**: optional short text.
- **Dismissed in a row**: a counter for suggestions.

## Build order

1. Lookout items and the 90-day fade (small, useful right away).
2. Comma splitting for dictation and pasting.
3. Start recording purchase dates **now**, even before suggestions exist, so there's history to learn from.
4. Suggested draft list.
5. MCP connection (after the online question is settled).

## Open questions

1. **Online or phone-only?** MCP needs the list stored online (a small private server synced to the phone). Upside: AI access, plus phone ↔ computer and sharing later. Downside: more to build and run, and your list leaves the phone. *My recommendation:* stay phone-only for the first Android version and add online sync together with MCP as step 5.
2. Should lookout items get a gentle nudge when you open a store view ("You're looking for Calabrian chili paste")? Or is the folded section enough?
