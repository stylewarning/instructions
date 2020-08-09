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