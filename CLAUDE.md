# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

- `npm install` — install dependencies (required on first checkout; `node_modules` is not committed).
- `npm run dev` — start the Vite dev server at http://localhost:5173.
- `npm run build` — produce a production build in `dist/`.
- `npm run preview` — serve the production build locally.
- `npm run lint` — run ESLint across the repo.

There is no test runner configured.

## Architecture

This is a single-page React 19 app bootstrapped with Vite. `src/main.jsx` mounts `<App />` in `StrictMode`. Styling is plain CSS in `src/App.css` and `src/index.css`.

Component layout (all in `src/`, flat — no `components/` directory):

- `App.jsx` — owns the `transactions` state (a `useState` array of `{ id, description, amount, type, category, date }`) and the hard-coded `categories` list. Renders the three children below; exposes `handleAdd` to append a new transaction.
- `Summary.jsx` — receives `transactions` as a prop and computes income/expense totals + balance internally. Amounts are stored as strings (raw `<input type="number">` values), so totals coerce via `Number(t.amount)` before summing.
- `TransactionForm.jsx` — owns its own form-field state (description/amount/type/category), calls `onAdd(transaction)` on submit, resets fields after.
- `TransactionList.jsx` — owns its own `filterType` / `filterCategory` state and renders the filtered table.

There is no persistence, routing, backend, or shared state library — the transactions list lives in memory only and resets on reload.

## Starter-code context

Per the README, this repo is the starter for a Claude Code course and intentionally shipped with bugs, weak UI, and messy code that are fixed during the course. The string-amount totals bug (`sum + t.amount` doing concatenation) has already been fixed in `Summary.jsx` via `Number(t.amount)`, but `amount` itself is still stored as a string on each transaction — any new code that does math with `amount` must coerce it. Other intentional rough edges (UI polish, code structure) may still be present; before "fixing" oddities as part of unrelated work, confirm with the user that it isn't intentional learning material.

## Tooling notes

- ESLint (`eslint.config.js`) uses the flat-config format with `@eslint/js` recommended, `eslint-plugin-react-hooks`, and `eslint-plugin-react-refresh` (Vite preset). The `no-unused-vars` rule ignores identifiers matching `^[A-Z_]` (uppercase / underscore-prefixed names).
- Vite + `@vitejs/plugin-react` require Node.js 20.19+ or 22.12+. Older Node versions print an engine warning but still run.
