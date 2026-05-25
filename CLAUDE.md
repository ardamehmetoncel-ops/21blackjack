# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the game

Open `index.html` directly in a browser — no build step, no dependencies.

## Architecture

Single-file vanilla JS (`index.html`). All logic is inline:

- **Shoe**: 7 decks (364 cards), Fisher-Yates shuffled. Cut card placed at a random position within the 4th deck (indices 156–207). `needsShuffle` flag is set when dealing past the cut card; the actual reshuffle happens at `newRound()`.
- **Game state**: One global `G` object mutated directly. Phases: `betting → playerTurn → botTurn → dealerTurn → done`.
- **Bot AI**: Full basic strategy (hard/soft/split tables) in `botDecide()`. Bots act sequentially via `setTimeout` chains (`runBotStep()`). Same for the dealer (`dealerStep()`).
- **Split**: Up to 3 splits (4 hands max). Split aces receive one card and auto-stand. `G.activeHandIdx` stays on the first split hand; `advancePlayer()` scans forward for the next `status === 'active'` hand.
- **Rendering**: `render()` regenerates all DOM from `G` on every state change. Card elements have a CSS pop-in animation.
- **Payouts**: Blackjack 3:2 (`hand.bet * 2.5`). Win returns `hand.bet * 2`. Push returns `hand.bet`. Doubled hands store the full doubled bet in `hand.bet` so payout math is uniform.

## Keyboard shortcuts (player turn)

`H` hit · `S` stand · `D` double · `P` split
