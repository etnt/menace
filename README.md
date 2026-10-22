# MENACE: A Reinforcement Learning Experiment

This project is a small web page that lets you play tic-tac-toe against
**MENACE**. MENACE is a machine that learns from games. It uses only
matchboxes and beads, and it needs no computer.

Play it online at: **https://etnt.github.io/menace/**

The original MENACE was built in 1961 by Donald Michie. Read more on
[Wikipedia](https://en.wikipedia.org/wiki/Matchbox_Educable_Noughts_and_Crosses_Engine).

This version runs in your browser. It is one file: `index.html`.

## How it works

- Each board position has a **matchbox**. Boards that are turned or mirrored copies of each other share one matchbox.
- A matchbox holds **beads**. Each bead is one possible move. Moves that are
  mirror copies of each other share one kind of bead.
- Each board position has its own matchbox. The box for the empty board
  starts with 4 beads of each colour. Boxes for positions MENACE meets later
  start with fewer: 3 on its second turn, 2 on its third, 1 on its fourth.
  After that, wins, draws and losses change the counts. (Michie's MENACE
  always moved first; this version uses the same pattern when MENACE plays
  second.)
- To play, MENACE picks a bead at random from the matchbox for the
  current position. More beads for a move means a higher chance to pick it.
- After each game, MENACE updates the beads it used:
  - **Win:** each move gets 3 more beads.
  - **Draw:** each move gets 1 more bead.
  - **Loss:** each move loses 1 bead.
- If a matchbox runs out of beads, MENACE **resigns**. The game counts as
  a loss, and its moves lose beads.

Over many games, MENACE learns to avoid bad moves and prefer good ones.

## What you can do

- **Play** against MENACE. Choose whether you use X or O.
- **Train** MENACE automatically. Run up to 1,000,000 games against a
  random opponent.
- **See what MENACE learned.** The matchbox view shows the beads for each
  move. A report after each game shows how the used matchboxes changed.
- **Check its skill.** The Learning Level panel shows how close MENACE
  is to perfect play. It also runs a test against a perfect opponent.
- **Reset** MENACE's memory to start again.

Learned data is saved in your browser (`localStorage`). It stays after you
close the page.

## Run it

No install is needed. Open `index.html` in a web browser.

You can also play it online at **https://etnt.github.io/menace/**.
The site is published with GitHub Pages. See the workflow in
`.github/workflows/deploy-pages.yml`.

## Files

- `index.html`: the whole app (HTML, CSS, and JavaScript).
- `plan/implementation-plan.md`: the plan used to build the app.
- `LICENSE`: the license.

## License

See [LICENSE](LICENSE).
