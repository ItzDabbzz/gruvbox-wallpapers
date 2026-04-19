# Gruvbox Wallpapers
A place where to find gruvbox theme wallpapers.
[https://gruvbox-wallpapers.pages.dev/](https://gruvbox-wallpapers.pages.dev/ 'Gruvbox Wallpapers')

## Contributions

1. Fork the project.
2. Add the wallpapers that you have in their respective folder.
3. For light variants: Place the light-themed wallpapers in a light/ subfolder within the category.
4. Make a pull request.

> [!CAUTION]
> - Use files under 25mb
> - Avoid spaces in names

> [!TIP]
> If your wallpaper does not fit in any of the folders we can debate the creation of a new one.

I tend to accept any contributions, but let's keep the site with images that match or look good with the gruvbox's color scheme :').


## Nix package

1. **Use [nix flakes](https://wiki.nixos.org/wiki/Flakes)**:

2. Add the following to your `flake.nix` file:
   
```nix
inputs = {
  gruvbox-wallpapers.url = "github:ItzDabbzz/gruvbox-wallpapers";
  # ...
};
```

3. Then, in your Home Manager configuration:

```nix
{
  inputs,
  pkgs,
  ...
}: {
  home.file = {
    "path/to/dir" = {
      source = inputs.gruvbox-wallpapers.packages."${pkgs.stdenv.hostPlatform.system}".default;
      recursive = true;
    };
  };
}
```

## CLI helper

This repo ships `bin/gruvwall`, a CLI helper for browsing, picking, and applying wallpapers.
Tested on Arch Linux with KDE Plasma Wayland and Hyprland.
Not Arch-specific. Install equivalent commands for your distro and use `--backend` when autodetect does not fit your session.

### Dependencies

Required:
- `bash`
- `findutils` (for `find`)

Optional (recommended):
- `fzf` (interactive picker)
- `fd` (faster file discovery)

Backends:
- KDE Plasma Wayland:
  - Preferred: `plasma-apply-wallpaperimage`
  - Fallback: `qdbus6` or `qdbus`
- Hyprland:
  - `swww` backend: `swww` (and run `swww init` once per session)
  - `hyprpaper` backend: `hyprpaper` + `hyprctl`
  - Optional: `jq` (better monitor detection)

Autodetect currently supports KDE and Hyprland. On other desktops/sessions, pass `--backend` explicitly.

### Install

From repo checkout:

```sh
install -Dm755 ./bin/gruvwall ~/.local/bin/gruvwall
```

Packaged installs should place wallpapers at `/usr/share/gruvbox-wallpapers/wallpapers` or next to script under `../share/gruvbox-wallpapers/wallpapers`.

Point helper at a different wallpaper directory with `GRUVWALL_WALLPAPER_DIR` or `~/.config/gruvwall/config`.

### Quick usage

From a repo checkout:

```sh
./bin/gruvwall --list
./bin/gruvwall --pick
./bin/gruvwall --random
```

To list everything (including light variants):

```sh
./bin/gruvwall --list --mode all
```

Force backend:

```sh
./bin/gruvwall --pick --backend kde
./bin/gruvwall --random --backend swww
./bin/gruvwall --set ./wallpapers/brands/firefox.png --backend hyprpaper
```

Print-only (choose, but do not apply):

```sh
./bin/gruvwall --pick --print
./bin/gruvwall --random --print
```

### Light / Dark mode

Categories may include a `light/` subfolder containing light-mode variants. Filtering is based on whether the wallpaper path contains a `/light/` path segment (case-insensitive).

`--mode auto` is the default:
- KDE Plasma: reads `~/.config/kdeglobals` (ColorScheme contains "Dark"/"Light")
- Hyprland: checks `gsettings org.gnome.desktop.interface color-scheme` (prefer-dark vs default) or `~/.config/gtk-3.0/settings.ini`

You can override with:

```sh
./bin/gruvwall --pick --mode auto
./bin/gruvwall --pick --mode light
./bin/gruvwall --pick --mode dark
./bin/gruvwall --pick --mode all
```

Environment override (applies when you do not pass `--mode`):

```sh
GRUVWALL_MODE=light ./bin/gruvwall --random
GRUVWALL_MODE=all ./bin/gruvwall --list
```

If you want to point at a different wallpaper directory or set default `swww` args, create `~/.config/gruvwall/config`:

```ini
# Where wallpapers live (optional)
wallpaper_dir=~/Pictures/gruvbox-wallpapers/wallpapers

# Optional: override swww transition args (split on whitespace)
swww_args=--transition-type grow --transition-duration 1
```

### Hyprland setup examples

`swww` (recommended):

```ini
# hyprland.conf
exec-once = swww init
```

Then:

```sh
./bin/gruvwall --random --backend swww
./bin/gruvwall --random --backend swww --swww-args "--transition-type wipe --transition-duration 1"
```

`hyprpaper`:

```ini
# hyprland.conf
exec-once = hyprpaper
```

Then:

```sh
./bin/gruvwall --pick --backend hyprpaper
```

## Disclaimer 

Most of the images shared here are community contributions, and their original sources are often unknown. If you are the rightful owner of any image and would like it removed, please contact me by opening an issue. For any use beyond personal scope, I recommend performing a reverse image search to verify the origin.
