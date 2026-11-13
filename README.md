[English](README.md) | [简体中文](README.zh-CN.md)

# DMF 48-Channel Controller

![Type: Course Project](https://img.shields.io/badge/Type-Course%20Project-64748B?style=flat-square)

A Python/PyQt5 desktop control application for digital microfluidic experiments, combining electrode-grid visualization, droplet path planning, chip testing and real-time serial control.

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue)](https://python.org)
[![PyQt5](https://img.shields.io/badge/PyQt5-5.15%2B-green)](https://pypi.org/project/PyQt5/)
[![License](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

Windows 10/11 installers are available from [Releases](https://github.com/Cavalcdor/dmf-48channel-controller/releases). See [USAGE.md](USAGE.md) for the user manual.

## Features

- **Three-pane interface:** controls and chip-test tabs on the left, electrode grid in the center, status and planning information on the right. Flat gray/blue styling and live connection/run indicators.
- **Serial communication:** port scanning, connection monitoring, custom commands and single-channel tests. Supported commands: `ON,ch`, `OFF,ch`, `ALLON`, `ALLOFF`, `TEST`, `LIST`, `HELP`. Virtual serial mode simulates responses without hardware.
- **Configurable grids:** 2×2 to 16×16, default 6×8. Displays idle cells, starts, destinations, obstacles, active droplet positions and chip-test coverage. Click to assign or cancel starts/destinations; switch modes to edit obstacles.
- **A* planning:** four-direction search with a Manhattan-distance heuristic. Plan up to eight droplets with greedy conflict avoidance and shared destinations for merging. Diagnostics distinguish blocked paths, occupied cells and identical starts/destinations.
- **Execution modes:** preview without sending commands, automatic execution or manual stepping. Delays are 100/200/500/1000 ms. Each step switches the current electrode on and the previous one off; Stop interrupts all relays.
- **Droplet management:** independent IDs and path colors, previous/next navigation, return to droplet 1, and separate clear-current/clear-all actions.
- **Chip testing:** serpentine coverage, A* rerouting around marked defective electrodes, automatic or manual stepping at 50–5000 ms intervals, pass/fail/untested and reroute counts, and TXT/CSV reports.
- **Editing and review:** up to 100 undo/redo steps with Ctrl+Z/Ctrl+Y, PNG grid export and clipboard copying, live keyword/severity log filtering, serial-command recording/replay and JSON export.
- **Waypoints:** right-click to place one or more diamond-marked waypoints for a droplet.
- **Completion sounds:** configurable notifications after path execution or chip testing.

## Quick start

```sh
pip install -r requirements.txt
python main.py
```

For normal operation:

1. Connect the serial port.
2. Set the grid dimensions and create a grid.
3. Select each droplet and assign its start (blue) and destination (orange).
4. Mark obstacles.
5. Preview the planned paths.
6. Run them automatically or step through manually.

For chip testing, connect the hardware, open the chip-test tab and start an automatic scan. Mark defective electrodes as encountered; the system replans around them. Export the report when finished.

Creating/resetting a grid returns to droplet 1. Grid dimensions are synchronized with the global configuration. Clearing obstacles affects only obstacle cells; clearing the current droplet affects only its start/destination; clearing all removes all droplet configurations. Chip-test defect marks are separate from normal-operation obstacles.

## Source layout

| Path | Responsibility |
| --- | --- |
| `main.py` | Main window, signals, execution and chip testing |
| `requirements.txt` | Dependencies |
| `assets/style.qss` | Legacy stylesheet; active styles are inline in `main.py` |
| `build/generate_icon.py` | Icon generation |
| `build/setup_installer.iss` | Inno Setup installer |
| `build/version_info.txt` | Version metadata |
| `src/global_cfg.py` | Grid and serial configuration |
| `src/grid_widget.py` | Grid rendering and interaction |
| `src/path_algorithm.py` | A*, multi-droplet planning, serpentine scans and rerouting |
| `src/serial_driver.py` | Threaded serial communication and asynchronous reception |
| `src/dashboard.py` | Dashboard |
| `src/about_dialog.py` | About dialog |
| `src/auto_update.py` | Updates |
| `src/log_panel.py` | Five-level colored logging |
| `src/project_manager.py` | JSON project persistence |
| `src/settings.py` | Settings dialog |

## License

[MIT](LICENSE).
