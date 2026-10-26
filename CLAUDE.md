# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

ForgeRig: a static storefront for PC parts and pre-built PCs. The whole site is one file, `index.html`, with inline CSS and JS. It has no build step, no dependencies and no package.json.

## Commands

- Run: open `index.html` in a browser.
- Syntax-check the script (Node):
  ```sh
  sed -n '/<script>/,/<\/script>/p' index.html | sed '1d;$d' > /tmp/s.js && node --check /tmp/s.js
  ```
- Tests: none. The only check is a `console.assert` on `cartTotal` at the end of the script; it runs on page load and reports in the browser console.

## Architecture (inside `index.html`)

- `PRODUCTS` is the whole catalog, hard-coded at the top of the script. Pre-built PCs have a `specs` array and parts have a single `spec` string; `renderGrid` relies on that difference. Categories in the filter bar come from the `cat` values in this list.
- State is two variables: `cart` (`{productId: qty}`) and `filter` (the selected category). After a state change, the matching `render*` function rebuilds its section with `innerHTML`.
- `renderCart` also saves `cart` to `localStorage`, wrapped in try/catch. Cart ids missing from `PRODUCTS` are skipped, so removing a product can't break a saved cart.
- One delegated click listener on `document` handles every button through `data-cat`, `data-add` and `data-dec` attributes.
- `money()` formats every price; the currency is set there (currently USD).
- Checkout is a demo: it shows a message and clears the cart. No payment is taken and no order is saved. It's marked with a `shortcut:` comment, so connect a real payment backend before treating the site as able to sell.

## Workflow rules (from the user)

1. **Models:** use Opus 5.5 for planning and Sonnet 5.5 to carry out the plan.
2. **After writing code:** test the code, and run regression tests too.
3. **After testing:** run a security review with the `security-review` skill.
4. **After every code change:** make a git commit.
5. **At the end of every agent task:** record each update in the Obsidian project note (`Projects/ForgeRig/ForgeRig.md`).
6. **Git/GitHub:** the project uses a git repo and a GitHub repo with two branches, `main` and `dev`.

## Related

Project notes are in the user's Obsidian vault at `Projects/ForgeRig/ForgeRig.md` (written in Hebrew; includes a to-do list).
