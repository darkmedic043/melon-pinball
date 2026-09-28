# melon pinball

A pinball table on the melon planet, with real ball physics: a 27 mm steel ball on a tilted table, flippers,
bumpers, slingshots and a wire ramp. All artwork and sound are generated when the game starts; there are no asset
files.

- **Classic:** five missions to complete for rank progression.
- **Harvest:** roguelike runs through 8 seasons of fields, with score targets, pests, and a Seed Market where you
  buy melons (modifiers) and grafts (temporary boosts).

It's the game [melon](https://github.com/melon-77/melon-os) ships on its desktop edition (`recipes/melon-pinball`
there builds and packages it), but it builds anywhere with SDL3.

## Building

Needs a C++17 compiler, CMake, and SDL3, SDL3_image and SDL3_ttf.

```sh
cmake -S src -B build -DCMAKE_BUILD_TYPE=Release -DPINBALL_TESTS=ON
cmake --build build
build/melon-pinball-physics-test && build/melon-pinball-game-test
build/melon-pinball
```

`PINBALL_DATADIR` (default `<prefix>/share/melon-pinball`) is where the game looks for `icon.svg` and an optional
`mascot.png`; `MELON_PINBALL_DATA` overrides it at run time. Text uses Noto Sans, DejaVu Sans or Hack when they're
installed, and a built-in dot-matrix font otherwise. `files/` holds the desktop entry and the icon for packagers.

## Tests

Both run headless, and melon's package build runs them, so a change that breaks the table or the rules fails the
package:

- `melon-pinball-physics-test`: launches, flipper and mini-flipper shots, cradles, the ramp, and 450 random balls
  that must never leave the cabinet.
- `melon-pinball-game-test`: a Harvest run through the Seed Market, melons and pests acting on the machine,
  kickback, spinner, magnet, and a lost run.

To look at the game without a screen:

```sh
SDL_VIDEO_DRIVER=offscreen SDL_RENDER_DRIVER=software build/melon-pinball --screenshot out.png --seconds 30 --play
```

The demo plays a classic game; add `--harvest` for a run, `--lazy` to lose it quickly; `--shop`, `--packs` and
`--collection` show those screens, and `--golden` the gauntlet survivor's look.

## Controls

Flippers: Z / Left Shift / Left and / / Right Shift / Right. Plunger: Space, Down or Enter. Nudge: X, `.` and Up.
F1 help, F2 new Harvest run, F5 classic game, F6 collection. Gamepads and the mouse work too.

## Finding your way around

The table's geometry lives in `src/table.cpp` and drives both the physics and the painted art; after moving
anything, check the physics test's "at rest mid-table" count (a ball that can come to rest off the flippers is a
trap). Harvest's melons, grafts, pests and seed packs are data in `src/run.cpp`; their effects are in
`src/game.cpp` (`applyMachine` for the physics, `add` and `currentMult` for the scoring). Unlocks are variety only
(no permanent power) and live in the player's `~/.local/share/melon/pinball/unlocks.txt`.

## License

MIT, see `LICENSE`.
