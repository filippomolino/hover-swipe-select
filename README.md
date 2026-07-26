# Hover Swipe Select

A GNOME Shell extension that brings a macOS-style gesture to the **Activities
Overview**: instead of clicking a window thumbnail to pick it, just **hover** it
with the touchpad and do a **3-finger swipe down** — that window is selected,
focused, and the overview closes.

![GNOME Shell 46](https://img.shields.io/badge/GNOME%20Shell-46-blue)

## What it does

- In the overview, **hover any window** and **3-finger swipe down** → that window
  becomes active and the overview closes (just like clicking it, but with a flick).
- If your swipe does **not** start over a window, nothing is intercepted — GNOME's
  normal overview gestures (swipe down to leave, swipe up for the app grid) and the
  horizontal workspace-switch gesture keep working exactly as before.

## How it works

GNOME owns the 3-finger vertical swipe in the overview via
`Main.overview._swipeTracker`, whose handler consumes the gesture once it passes a
16 px threshold. Rather than racing that handler, this extension **disables the
overview's vertical swipe tracker only for the single gesture that begins over a
window**, handles the whole swipe itself, then restores the tracker on gesture end.
That makes the behaviour reliable regardless of how fast you swipe, and leaves every
other gesture untouched.

## Requirements

- GNOME Shell **46**
- A touchpad that reports multi-finger swipe gestures (works on both Wayland and X11)

## Installation

### From extensions.gnome.org

Search for **“Hover Swipe Select”** on <https://extensions.gnome.org> and toggle it on.

### Manual

```sh
git clone https://github.com/Amgadezzat-andro/hover-swipe-select.git
cp -r hover-swipe-select ~/.local/share/gnome-shell/extensions/hover-swipe-select@amgad
```

Then **log out and back in** (required on Wayland to load a new extension) and enable it:

```sh
gnome-extensions enable hover-swipe-select@amgad
```

## Usage

1. Open the Activities Overview (Super key, or swipe up with 3 fingers).
2. Move the pointer over the window you want.
3. Swipe **down** with 3 fingers.

## Tweaking

Two constants at the top of [`extension.js`](extension.js):

- `TRIGGER_DISTANCE` — how far you must swipe down before it selects (default `30`).
- `DIRECTION_MIN` — movement needed before the swipe axis is locked in (default `8`).

Set `DEBUG = true` and watch the log to see what it detects:

```sh
journalctl -f -o cat /usr/bin/gnome-shell | grep hover-swipe-select
```

## License

[GPL-2.0-or-later](LICENSE)
