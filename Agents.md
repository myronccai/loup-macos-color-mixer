# Agents.md — loup-macos-color-mixer

A sample macOS GUI application written in Loup: a color mixer with RGB / HSB
sliders and numeric fields, a hex field, a color well, a Copy Hex button, and a
large swatch plus five harmony swatches drawn on a canvas. It is built on the
`appkit` proxy library, [loup-macos-appkit](https://github.com/myronccai/loup-macos-appkit),
which must be checked out beside this repository at `../loup-macos-appkit`
(the manifest's path dependency).

Start with [README.md](README.md): what the app does, how to build, run and
test it, the self-test, the file layout and the appkit modules it uses.

## Sources

- `src/main.lphc` (`@mode(host.native)`) — the UI and the `--self-test`
  driver. Read its header comment first: `apply(mixer, source)` is the one
  place controls are written. It uses only appkit proxies (no `appkit.objc`);
  keep it that way, and if the library lacks something, comment the raw call
  with why. It carries no toolchain workarounds.
- `src/colormodel.loup` (`@mode(host.dual)`) — the model and all color logic;
  no AppKit, so it runs under `loup run` / `loup test`. Put new logic here.
- `src/colormodel_test.lpscr` — `loup test --strict src`.
- `selftest.out` — the golden output of `./loup-macos-color-mixer --self-test`;
  regenerate it only for an intended change, and check both drivers agree.

## Reference

- The collection: the appkit repository's [README.md](https://github.com/myronccai/loup-macos-appkit/blob/main/README.md)
  and [Agents.md](https://github.com/myronccai/loup-macos-appkit/blob/main/Agents.md)
  describe the library and all the apps built on it.
- appkit library: `../loup-macos-appkit/*.lphc` (each module's header comment) and
  `../loup-macos-appkit/docs/` (canvas, dialogs, table); working examples of every
  module in `../loup-macos-appkit/probes/*_probe/src/main.lphc`.
- The gate: `bash ../loup-macos-appkit/run_all.sh [--screenshots] loup-macos-color-mixer`.
- macOS + Loup: the bundled Loup macOS developer skill,
  `../loup-macos-appkit/skills/loup-macos-developer-skill/SKILL.md` (and its
  `references/`), in the sibling appkit checkout.
- Toolchain: Loup newer than 1.27.0 (`loup`, `loup-build`, `loup-llvm`,
  `loup-test`, `loup-bootstrap` on `PATH`); see README.md's Getting started.

## Rules for working here

- Launch the binary only with `--self-test`, through the gate or `lldb`
  (`lldb --batch --no-lldbinit -o "process launch -- --self-test" -k "thread backtrace" -k "process kill" -- ./loup-macos-color-mixer`).
- The self-test must drive the app through its controls and print only
  deterministic values; wrap clipboard use in `saveClipboard` /
  `restoreClipboard`.
- Leave no build litter (`.temp/`, `.loup-cache/`, the binary,
  `*.loupbuild.fp`; see `.gitignore`).

## License

MIT; see [LICENSE](LICENSE).
