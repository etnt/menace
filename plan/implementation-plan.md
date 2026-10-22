# MENACE Interactive Learning Experiment — Implementation Plan

## Goal

Build a polished, self-contained web learning experiment that lets a learner play tic-tac-toe against MENACE (Matchbox Educable Noughts and Crosses Engine) and see how its matchboxes change from experience. Deliver the experience as one HTML page with its CSS and JavaScript inline. It must run locally by opening the HTML file, with no build step, server, framework, or external dependency.

## Repository context

- The repository currently contains only a short project README and the MPL 2.0 license; there is no application code, framework, test setup, or existing design to preserve.
- Proposed deliverable: `index.html` at the repository root. Keep the experiment self-contained in that file.
- Preserve the project license and do not add dependencies or remote assets.

## Experience and interaction

1. Create an inviting, responsive “MENACE lab” interface with a clear title, a short plain-language explanation, and a playful visual identity inspired by matchboxes, beads, and paper game boards.
2. Make the 3×3 board the main interaction. Let the learner choose who moves first (human or MENACE), choose a difficulty/training opponent if feasible, and play an entire game with clear turn, win, loss, and draw feedback.
3. Show MENACE’s current decision in an adjacent “matchbox” panel: the selected board state, legal next moves, and the number of beads assigned to each move. Animate or highlight the chosen move and bead changes, but respect reduced-motion preferences.
4. Explain the learning update after each completed game. Show a session counter and outcome totals so learners can connect repeated play to behavior change.
5. Add controls to train several games automatically against a simple documented opponent, pause/step where practical, reset MENACE’s learned experience, and start a new game. Keep manual play usable without using the training controls.
6. Persist learned matchboxes and session stats in `localStorage`, with safe handling for missing, malformed, or unavailable storage. Provide an explicit reset so the user can start over.

## MENACE model and rules

- Represent a board as nine cells with empty, X, or O values. Track the current player, game state, legal moves, and outcome independently of the UI.
- Treat MENACE’s mark as a configurable player mark based on who starts. Store a matchbox per encountered board state from MENACE’s perspective; each legal move has a non-negative bead count. A stable board key should include cell contents and whose turn it is, or otherwise guarantee the same state cannot be confused across turns.
- Select a legal move by weighted random sampling from that state’s bead counts. If all counts reach zero, use a uniform random legal move so MENACE can continue exploring instead of becoming stuck.
- Record every move MENACE makes in the current game. After the game ends, reinforce only those recorded decisions: add 3 beads per selected move for a MENACE win, add 1 for a draw, and remove 1 (floor at zero) for a loss. State this simplified reward scheme in the interface; avoid implying that the agent uses a neural network or gradient-based training.
- Validate wins across all rows, columns, and diagonals; detect draws and prevent moves after the game has ended. Keep a deterministic/testable seam for move selection or reward updates if practical, while leaving normal play stochastic.
- For automated training, use a clearly described baseline opponent (for example, random legal moves) and expose the number of games trained. Do not silently claim self-play or use a perfect opponent unless implemented.

## Suggested implementation structure inside `index.html`

- Inline `<style>`: visual system, responsive layout, board and matchbox states, focus states, reduced-motion rules.
- Semantic HTML: header/intro, game controls, board, status/live announcement, matchbox visualization, learning explanation, session stats, reset confirmation.
- Inline `<script>` sections/functions, separated by comments:
  1. Constants, initial state, persistence load/save.
  2. Pure game rules: legal moves, win/draw detection, state key.
  3. MENACE: initialize matchbox, weighted choice, move history, reinforcement.
  4. Game/session actions and automated training.
  5. Rendering and event wiring.
- Keep game state authoritative in JavaScript; render the board and matchbox from state rather than mutating UI independently.

## Accessibility, responsive design, and safety

- Use buttons for board cells and controls; include readable accessible names (row/column and occupant), visible keyboard focus, and keyboard-operable interaction.
- Announce turn changes and outcomes through an `aria-live` status region. Do not rely on color alone to distinguish X and O or selected moves.
- Support narrow screens without horizontal scrolling; stack the matchbox and board panels on mobile.
- Use no external fonts, images, libraries, network calls, or backend. Handle storage failures without blocking play.

## Aesthetic and Feature Refinements

These refinements elevate the learning experience from a purely functional demonstration to a delightful, interactive experiment.

**Visual Design:**
- Adopt a matchbox and bead theme with a warm paper palette and use rounded cards for all elements.
- Ensure X and O have distinct visual shapes.
- Implement a dynamic bead-pop animation upon state transition, ensuring compliance with the `prefers-reduced-motion` media query.

**Learning Visuals:**
- Display a bead-count bar chart alongside the board to visualize the reward distribution for the current move.
- Implement a win-rate-over-time sparkline (using inline `<canvas>` or SVG) to show MENACE's performance evolution over the last N training games.

**Fun Touches:**
- Incorporate MENACE speech bubbles to provide contextual, conversational feedback during gameplay.
- Add a speed slider that allows the user to adjust the training volume (e.g., 1, 10, or 100 games at a time) for automated training sessions.

**Symmetry (Stretch Goal):**
- While the default implementation will not use symmetry reduction for simplicity, the canonicalization of the 8 board symmetries should be considered as a stretch goal to reduce the required state space (from ~304 unique configurations).

- Open `index.html` directly in current desktop and mobile-sized browsers; confirm no console errors and no network requests.
- Check all eight win lines, draw, legal-move handling, and terminal-state lockout.
- Verify weighted sampling favors moves with more beads, zero-bead fallback remains legal, and win/draw/loss updates apply the documented +3/+1/−1 rule to every MENACE move in the game.
- Play with both starting-player choices and confirm the matchbox records MENACE’s own decisions only.
- Run auto-training for multiple games and verify game/outcome counters and visible bead counts change; pause/reset behavior must not corrupt an in-progress game.
- Reload and confirm experience persists; test malformed storage and unavailable `localStorage`; confirm explicit reset clears saved data.
- Check keyboard-only use, screen-reader status announcements, reduced motion, and narrow viewport layout.

## Acceptance criteria

- A single root `index.html` contains the complete interactive experience and works without setup or external resources.
- A user can play tic-tac-toe against MENACE and inspect the selected matchbox and its move weights.
- Completed games update MENACE’s bead counts according to the displayed reward rule, and automatic training provides repeated experience against the disclosed baseline.
- The experience communicates what MENACE learns, persists it locally, and offers a working reset.
- The interface is responsive, keyboard accessible, and handles errors in local persistence gracefully.

## Detailed design requirements (next step)

- **Visual Design:** Adopt a matchbox and bead theme with a warm paper palette and rounded cards. X and O must have distinct visual shapes/colors. Implement a dynamic bead-pop animation upon state transitions, ensuring compliance with the `prefers-reduced-motion` media query.
- **Learning Visuals:** Display a bead-count bar chart alongside the board to visualize the reward distribution for the current move. Implement a win-rate-over-time sparkline (using inline `<canvas>` or SVG) to show MENACE's performance evolution over the last N games.
- **Fun Touches:** Incorporate MENACE speech bubbles to provide contextual, conversational feedback during gameplay. Add a speed slider that allows the user to adjust the training volume (e.g., 1, 10, or 100 games at a time).
- **Symmetry (Stretch Goal):** While the default implementation will not use symmetry reduction for simplicity, the canonicalization of the 8 board symmetries should be considered as a stretch goal to reduce the required state space.
