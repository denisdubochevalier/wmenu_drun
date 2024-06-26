# WMenu in Rust
This repo contains a wrapper around wmenu(1), inspired by Nathaniel Maia's
`wmenu_drun` script
(https://forum.archlabslinux.com/t/a-desktop-file-scrubber-and-launcher-for-dmenu/1008),
and Tom van Dijk's `dmenu_drun` program (https://github.com/dtomvan/dmenu_drun).
It also shows .desktop files, similar to rofi. You could see it as suckless
rofi. This is not a rewrite of WMenu in Rust. WMenu is fast enough by itself,
and there are already some excellent patches to really make it your own.

## Why rewrite it in Rust?
Speed. I changed the original script to use the .desktop files title.
That slowed the script down by a lot. It takes .5 seconds to load, which is not
enough for me as a full-time rofi user to make the switch. Rofi is almost
instantaneous, because of the caching it performs. `dmenu_path` also caches,
which makes it start up very fast. I cannot use `stest` though, because I need a
key-value storage (title → desktop filename).

## Why use `gtk-launch`?
There are some quirks in .desktop files, it's easier to shell out to gtk. Most
people have it installed anyway.

## Installation
Dependencies:
- rustc 1.59.0-nightly (e012a191d 2022-01-06)
- wmenu
- gtk-launch (in gtk3) (optional: for desktop files)
Install dependencies using:
```bash
sudo pacman -S rustup wmenu gtk3
rustup toolchain install nightly-2022-01-06
```
NOTE: `pacman` commands only work on Arch Linux, find the appropriate commands
for your distro in your distro's documentation. Windows and Mac aren't
supported, as WMenu doesn't work on those OSes.

Install using:
```bash
git clone https://github.com/denisdubochevalier/wmenu_drun
cargo install --path wmenu_drun
```

# Usage
Just call `wmenu_drun`.
Use `-d` to exclude desktop files (if you didn't install gtk-launch).
Use `-p` to exclude `$PATH`.
By default, both desktop files and `$PATH` are enabled.

# TODO
Maybe in the future I will include these features:
- `-l [lang]` flag for localization.

# Localization
Localization in desktop files works as follows:
    - Use the user's locale from `$LANG` (the one in your /etc/locale.conf)
    This locale will be used to find localized names (if possible) in the
    .desktop file.

    See also: https://specifications.freedesktop.org/desktop-entry-spec/latest/ar01s05.html
