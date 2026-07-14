# ClusterForce Performance Benchmarks

Public collection point for results of the **in-game benchmark** shipped with the
ClusterForce client. The numbers tell us which machines run the game well, which do not,
and what actually costs frame time on them. That is what we tune the game against.

## How results get here

The benchmark never sends anything on its own. Nothing is uploaded in the background, and
there is no upload endpoint, no account link and no token in the game.

At the end of a run the result screen offers **"Ergebnis beitragen (GitHub)"**. Pressing it
opens GitHub's own web editor in the player's browser, with the report **already filled in**
as text -- the report travels inside the link, so there is no file to upload and nothing to
pick from disk. GitHub then forks this repository for the player and turns their click on
**"Propose new file"** into a pull request. Closing the tab sends nothing.

So: **consent is the click, and the click happens in the player's own browser.** A player who
never presses it never contributes anything.

Contributions land in [`reports/`](reports/) as one JSON file per run, named
`<timestamp>-<gpu>-cf<score>.json`.

## What a report contains

Hardware and graphics settings, plus the measured frame times. Every field is needed to read
the score at all -- a run at 67 % render scale, or a lean build flying two glider models
instead of sixteen, is a *different measurement*, not a faster machine.

| Field | Example |
| --- | --- |
| `cfScore` | overall score, `97` |
| `benchmark` | benchmark version -- staging and score formula change over time, `2` |
| `client`, `engine` | `2026.07.10`, `Godot 4.7-stable` |
| `gpu`, `gpuVendor`, `gpuApi` | `NVIDIA GeForce RTX 3070`, `NVIDIA`, driver/API version |
| `cpu`, `cpuThreads`, `ramMb` | `AMD Ryzen 7 5700X3D 8-Core Processor`, `16`, `65456` |
| `os`, `window`, `refreshHz` | `Windows`, `1920x1080`, `120` |
| `renderer`, `fullscreen`, `vsync` | `forward_plus`, `false`, `true` |
| `settings` | render scale, MSAA, clouds, HD models, ocean quality, SSIL |
| `fleetSkins` | how many glider models the build shipped (lean vs full) |
| `scenes` | per scene: `key`, `venue`, `mode`, `gliders`, `chainBalls`, average FPS, 1 % low, 0.1 % low, frame times (`msAvg`, `msMedian`, `msBest`, `msWorst`), `stutters` (frames over 2x the median), `frames`, `score` |
| `costs` | cost analysis per venue: `key` of the feature switched off, `base` and `fps` FPS |
| `secondsPerScene`, `warmupSec`, `costSeconds`, `time` | run timing and UTC timestamp |

The scene entries carry what was STAGED, not just what came out: how many gliders flew, how
many balls they hauled, which venue, which mode. A score without that context is not
comparable -- and neither is one from a different `benchmark` version.

## What a report does NOT contain

The data is **anonymous by construction** -- these things are never collected, so they cannot
end up in a report:

- no player name, nickname, account or in-game identity
- no system, host or machine name
- no operating-system user name
- no licence keys, serial numbers or hardware IDs
- no file paths, IP addresses, or location data

`gpu`, `cpu` and `os` describe the *hardware class*, not a person. If you would still rather
not share a run, do not press the button -- the game works exactly the same either way.

## What we do with it

Reports are collected **only to make the game run better**: finding GPUs that fall off a
cliff, checking whether a graphics feature is worth its frame time, and setting sane default
quality presets. They are not used for anything else, and they are not sold or passed on.

Pull requests are **picked up, evaluated and closed automatically on a schedule**. A closed
pull request does not mean your run was thrown away -- it means it has been read. Because
this is a public repository, every contributed report stays publicly visible in the pull
request; that is deliberate, and it is the reason nothing personal goes into one.

## Contributing without the button

You can add a report by hand: take `benchmark-latest.json` from the client's user directory
(Windows: `%APPDATA%\ClusterForce\Client\`) and open a pull request that adds it under
`reports/`. Please do not edit the numbers -- an edited report is worse than no report.
