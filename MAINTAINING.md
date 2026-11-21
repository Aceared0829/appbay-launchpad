# MacTahoe/CachyOS maintenance

This fork tracks the full-screen AppBay Launchpad and keeps a small
CachyOS/MacTahoe integration branch on top of upstream.

## Remotes

```bash
git remote -v
# fork      https://github.com/Aceared0829/appbay-launchpad.git
# upstream  https://github.com/nicolasdesenv/appbay-launchpad.git
```

## Updating from upstream

```bash
git fetch upstream
git switch mactahoe-cachyos
git merge --ff-only upstream/main
```

If upstream changed a file touched by this branch, resolve the conflict and
keep the MacTahoe defaults in `package/Plasma.AppBay/contents/config/main.xml`.

## Local install

```bash
kpackagetool6 -t Plasma/Applet -u package/Plasma.AppBay
cp icons/*.svg ~/.local/share/icons/hicolor/scalable/apps/
mkdir -p ~/.local/share/kwin/effects
cp -r kwin-effect/launchpadzoom ~/.local/share/kwin/effects/
kwriteconfig6 --file kwinrc --group Plugins --key launchpadzoomEnabled true
systemctl --user restart plasma-plasmashell.service
```

The panel integration is intentionally not automated here. It is applied
with Plasma scripting so the existing MacTahoe top bar and Dock remain intact.

## Current MacTahoe layout

- Top bar: widget `58` (`Plasma.AppBay`), leftmost, icon `start-here`,
  global shortcut `F4` (`activate widget 58`).
- Bottom Dock: widget `59` (`Plasma.AppBay`), leftmost, icon
  `appbay-launchpad`; panel order is `59;31`.
- Both instances use 7x5 pages and `systemActionsButtons=false`.
- Mouse wheels that report both `pixelDelta` and `angleDelta` are handled as
  discrete notches; one notch flips one page and fast wheels can skip pages.
- Page animation is 200 ms for one page, +70 ms per extra page, capped at
  420 ms (`Easing.OutCubic`).

## Uninstall

```bash
kpackagetool6 -t Plasma/Applet -r Plasma.AppBay
rm -rf ~/.local/share/kwin/effects/launchpadzoom
kwriteconfig6 --file kwinrc --group Plugins --key launchpadzoomEnabled false
systemctl --user restart plasma-plasmashell.service
```
