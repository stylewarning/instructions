# ThinkPad

These instructions apply to a vanilla Debian install.

## sbin

`/usr/sbin` isn't in your path by default, and a lot of important
tools end up there.

## Audio & Audio Controls

You need Alsa.

```
sudo apt install alsa-utils
```

To set the keys, you need `xbindkeys`:

```
sudo apt install xbindkeys
```

From there you can configure `~/.xbindkeysrc` to notice the audio buttons.

```
"amixer set Master 5%+"
   XF86AudioRaiseVolume
   
"amixer set Master 5%-"
   XF86AudioLowerVolume
   
"amixer set Master toggle"
   XF86AudioMute
```

You can load these with `xbindkeys --poll-rc`.

## Disable Forward/Back Keys

First put the xmodmap config in a file.

```
xmodmap -pke > ~/.Xmodmap
```

Then activate it inside of `.xinitrc`:

```
[[ -f ~/.Xmodmap ]] && xmodmap ~/.Xmodmap
```

Then disable the buttons by editing `.Xmodmap`:

```
! keycode 166 = XF86Back NoSymbol XF86Back
keycode 166 =
! keycode 167 = XF86Forward NoSymbol XF86Forward
keycode 167 =
```

Here, `!` is a comment. Refresh by running `xmodmap ~/.Xmodmap`.

## Keyboard Backlight

Echo `0`, `1`, or `2` to

```
/sys/class/leds/tpacpi\:\:kbd_backlight/brightness
```

Put these in scripts somewhere useful.

