This file contains instructions for using a DEC VT420, particularly on
macOS 10.12. This file was typed on a VT420.

This file also contains information about the VT520, which is a far superior terminal.

# VT420 vs VT520

Here are the important differences:

* The VT420 has _no_ hardware flow control. This is a huge pain for Emacs users.
  * The VT520 has DSR/DTR flow control, and RTS/CTS flow control on newer versions of firmware.
* Getting an "international edition" VT420 is a crapshoot. The "North American" edition only has DEC's weird MMJ adaptors on the back.
  * The VT520 has DB25 ports standard.
* Getting the keyboard configured is difficult on the VT420, requiring you to read the manual and send archaic key definition control sequences.
  * A key editor is included in the setup screen of the VT520.
* The VT420 requires proprietary keyboards over a proprietary (but probably easily reverse-engineered) interface.
  * The VT520 uses PS/2 and with enough patience can be set up to use just about any standard PS/2 keyboard. (*Note*: I have not set up a non-DEC keyboard yet. Things like Fn keys require massaging.)
* The VT520 can work at a blazing fast speed of 115.2K baud!

# THE KEYBOARD

The LK201 is awful. Typing on it for more than a few minutes will make
your hand hurt. The keys are mushy and difficult, and the caps-lock
key doesn't differentiate itself from the 'A' key.

Unfortunately, the keyboards use a proprietary protocol and interface,
so you don't have many options. The best I've found so far is the
LK402. The keys are noticeably better and the right side of the
caps-lock key is sufficiently differentiated from the 'A' key.

The situation with the 'alt' key is pretty dire. Don't expect to be
using it.

For the VT520, the state of affairs is vastly superior. The VT520
supports some PS/2 keyboards, but requires configuration. I've found
the IBM Model M `1391401` to work relatively well. On such a keyboard,
to get into settings, type `<CAPSLOCK>+<PRINTSCREEN>`.

# CONNECTING TO A MODERN COMPUTER

I use an FDTI-drivered USB-to-serial cable, because the Mac has native
drivers for these. There are at least two variants of the VT420 that I
own:

1. The "international" edition
2. The "North American" edition.

The international edition contains a convenient DB25 port on the
back. To connect to this port, you need the following chain:

```
USB-to-DB9 (male) <--> (female) DB9-to-DB25 (female) null modem
                  <--> (male) DB25 port on VT450
```

The North American edition doesn't have a DB25 port (or a removable
power cable!) and requires one of these special MMJ cables along with
special MMJ adapters. Here, I use:

```
USB-to-DB9 (male) <--> (female) DB9-to-DB9 (male) null modem
                  <--> (female) DB9-to-MMJ (female) DEC H8571-B
                  <--> (male) MMJ-to-MMJ (male)
                  <--> (female) MMJ port on VT450
```

The VT520 is all the same, unless you want to use DSR/DTR flow control. In that case, somewhere in the chain, you need to make the following crossover connections:

```
COMPUTER     TERMINAL
  DTR -------- RTS
  RTS -------- DTR
  DSR -------- CTS
  CTS -------- DSR
```

I am using a breakout box to do this conversion.

## Hardware Flow Control

Hardware flow control is *not* supported on the VT420. You will have to either set the baud rate to be low, or use XON/XOFF flow control.

The VT520 does, however, have hardware flow control. Depending on your firmware verson, you will have different options.

The VT520 with firmware V2.1 only has DSR/DTR flow control. As I understand, this is non-standard, and you will need to rig your cabling to pass DTR to RTS and DSR to CTS.

With other VT520 firmwares, you can do RTS/CTS flow control, which Just Works (TM).

# VT420 SETTINGS

Pressing F3 will get you into the menu. I suggest wiping it to factory
default first, and then changing the settings. We go through the
settings pages one-by-one.

## Global

Here you'll set up which port you're using. This is important on the
international edition, where you should set 'S1=Comm1' and
'Comm1=RS-232'.

## Display

Set the display how you'd like. I'm OK with 80x24 myself, but you
might want something different. Do change scrolling to "Jump Scroll"
and don't make your cursor blink.

If you want to use the largest setting possible, 132x48, then you will
need to change both of the respective settings, as well as the "pages"
setting to "3x48 pages". If you don't make this last setting, then
your terminal will only work for the upper half (24) lines of the
screen.

## General

- Set mode to VT400 Mode, 7 Bit Controls
- Use 8-Bit Characters
- I use UPSS ISO Latin-1
- ID as VT420.

## Comm

This is maybe the most important section if you want to get
connectivity with your computer.

- Transmit = 19200
- Receive = Transmit
- No XOFF
- 8 Bits, No Parity
- 1 Stop Bit
- Data Leads Only

Turning on XOFF will enable _software_ flow control.

## Printer

Nothing of note. Do you really want to be taking "screenshots" of your
terminal to a line or dot matrix printer?

## Keyboard

Mostly user preference here.

- Make `~ read as ESC.
- Make <> read as `~.
- Make the key click low.
- Disable F2
- Remember that shift-, and shift-. give you your beloved <> in case
  you've forgotten because you're using an LK201 keyboard.

## Tab

Nothing of note here.

# VT520 SETTINGS

## Display

Here we configure for 50x132

* Lines per screen: `48, 50, or 53`
* Lines per page: `50 lines x 02 pages`
* Columns per page: `132 Columns`
* Status display: `None` (you don't want screen burn-in do you?)
* Scrolling mode: `Jump`
* CRT Saver: `5 minutes`
* Energy Saver: `5 minutes`

## Terminal type

* Emulation mode: `VT520, ...`
* Terminal ID to host: `VT520`
* VT default char set: `ISO Latin-1`

## Keyboard

* Keyclick volume: low or off

For Emacs usage, there are a good number of keys you'll want to define. You'll want to use the command `sed -n l` on your host computer to determine what keycodes work. Assuming you have a keyboard like the LK412-AA, you might opt for the following customizations:

* Press `Alt`: Function
  *  `ESC` (for all fields)
* Press `<left>`: Function, define the following UDK sequences
  * Unshifted: `ESC[D`
  * Shifted: `ESC[1;2D`
  * Control: `ESC[1;5D`
  * Shift Control: `ESC[1;6D`
* Press `<right>`:
  * Unshifted: `ESC[C`
  * Shifted: `ESC[1;2C`
  * Control: `ESC[1;5C`
  * Shift Control: `ESC[1;6C`
* Press `<up>`:
  * Unshifted: `ESC[A`
  * Shifted: `ESC[1;2A`
  * Control: `ESC[1;5A`
  * Shift Control: `ESC[1;6A`
* Press `<down>`:
  * Unshifted: `ESC[B`
  * Shifted: `ESC[1;2B`
  * Control: `ESC[1;5B`
  * Shift Control: `ESC[1;6B`
* Press `<delete>` (not backspace!)
  * All: `ESC[3~`


## Communication

* Port Select... > 1 on comm1 (should be standard)
* Word Size: 8
* Parity: None
* Stop bits: 1
* Transmit speed: 115.2K
* Receive speed: Transmit speed
* Transmit Flow Control: prefer CTS, but DSR is OK if you have the cabling
* Receive Flow Control: prefer RTS, but DTR is OK if you have the cabling

# FIND THE SERIAL DEVICE

Run

    ls /dev | grep usb

and you should find something like `cu.usbserial-*` or
`tty.usbserial-*`. Whatever it is, remember it, because that's the
device we will be talking to. (You'll probably have both. Prefer the
`cu.*` variant.)


# TESTING CONNECTIVITY

To test connectivity, you'll want to use `screen` or, on a Mac,
CoolTerm.

    screen /dev/cu.usbserial-xxx 19200 cs8 ixoff -L

Make sure to replace `19200` with whatever baud you've selected before.

After running this, you should be able to type in your virtual terminal and have the characters show up on the real terminal. If they don't, that suggests there's an issue with the physical connection. Make sure you have a null modem in the loop, as suggested above. You can't just gender-change through this one!

To get out of screen, do `C-a-\ y`.


# STARTING A SESSION ON THE TERMINAL

Now that we've established connectivity, we can start a session. We need to set some things up on the Mac side.

The usual way to start a session is to use `getty`. Mac doesn't allow this unless it is within a launch daemon.

Create a file

    /Library/LaunchDaemons/serial-console.plist

and in it put the following

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>KeepAlive</key>
    <true/>
    <key>Label</key>
    <string>serial-console</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/libexec/getty</string>
        <string>std.19200</string>
        <string>cu.usbserial-A9080MNN</string>
    </array>
</dict>
</plist>
```

Note the string with `cu.usbserial-*`. Put whatever you got in there.

The `std.19200` will be looked up in `/etc/gettytab`. If you have additional customization, put it there, but don't modify `std.19200`, just make a new entry. I use this for the VT520.

```
# Serial over USB
std.ttyUSB:\
	:np:hw:mb:sp#115200:
```

Once you have this file (which you'll needed to have made as root)

    chown root:wheel /Library/LaunchDaemons/serial-console.plist
    sudo launchctl load -w /Library/LaunchDaemons/serial-console.plist
    sudo launchctl start serial-console

Now you should have an active terminal on your VT420.


# CONFIGURING THE SHELL

You'll want to tell your shell about your terminal personality (from
termcap/terminfo database). You can do this by exporting
`TERM=vt420`. Exporting an earlier compatible term is also fine, like
`TERM=vt220`.

For some easy setup with the terminal as 132x36, run

```
source vt420.sh
```

provided in this directory. This also sets the locale to ISO8859-1.

We also provide a `vt520.sh`.

To do this automatically upon login, add this to your `.bash_profile` (not `.bashrc`!):

```
export TTY=$(tty)
if [ "$TTY" = "/dev/cu.usbserial-..."]; then
  echo "Logging in from the USB serial port."
  source /path/to/vt520.sh
fi
```

# TMUX

`tmux` by default is pretty noisy. I use the config `vt-tmux.conf` and I add the alias

```
alias vtmux="tmux -f /path/to/vt-tmux.conf"
```

to my `.bashrc`.

# EMACS

Emacs has a special-purpose facility for loading terminal-specific stuff. Here are some notes.

## Loading Terminal-Specific Nonsense

If your terminal is set by `$TERM`, then Emacs will try to `load` a
file named `term/$TERM.{el,elc}`. However, aliases that Emacs has seem
to supersede the `$TERM`. It turns out that the variable
`term-file-aliases` contains these, and it sets the alias `vt420` as
for `vt200`. Therefore, to make terminal-specific stuff, either remove
this alias and create the file `term/vt420.el` or keep the alias and
create the file `term/vt200.el`.

You can detect whether you're in a non-graphical terminal in Emacs by
doing:

    (setq in-terminal (not window-system))


## Getting Fonts & Shift+Arrow To Work

When using Emacs with `tmux`, Emacs modifier keys Just Work. However,
things like underlining do *not* work. The reason for this is that
`tmux` by default sets `$TERM` to `screen`, which doesn't have the
terminfo database entries for telling applications how to
underline. (You can inspect the terminfo database entry by doing
`infocmp -1 $TERM`.)

However, if you set `TERM=vt520` before you run `emacs`, then modifier
keys don't fully work (like `<S-right>`). You'll see this because the
control keys error and insert garbage (e.g., `2C`) into the
buffer. The reason is that Emacs has special handling of `screen`,
which falls back to `xterm` initialization, which does a lot *within*
Emacs to make the editing experience great. One such thing it does is
add to the `input-decode-map` for recognizing and consequently
handling these ANSI control sequences.

To fix this, add to `term/vt520.el` the following lines:

```
(define-key input-decode-map "\e[1;2A" [S-up])
(define-key input-decode-map "\e[1;2B" [S-down])
(define-key input-decode-map "\e[1;2C" [S-right])
(define-key input-decode-map "\e[1;2D" [S-left])

(define-key input-decode-map "\e[1;5A" [C-up])
(define-key input-decode-map "\e[1;5B" [C-down])
(define-key input-decode-map "\e[1;5C" [C-right])
(define-key input-decode-map "\e[1;5D" [C-left])
```


## Emacs Tips

Don't load any of the fancy color theme stuff you might have. That can
both confuse the terminal, as well as make it much slower (because it
has a lot more to interpret).

## Things That Don't Work

- The alt key doesn't work on VT420. It can be configured as escape on VT520.
- On the VT420 without software flow control and on max speed, scrolling down using arrows causes a lot of garbled text for me.
- A lot of key commands don't work (probably because the terminal doesn't know how to send them). For example:
  - `C-right` (works on VT520 w/above config), `C-)`
  - `C-_` (use `C-/` instead; it's also easier anyway!)
- On the VT520 with DSR/DTR flow control, flow control seems to not work when in a `screen` session.


# TODO ITEMS

- TODO: Figure how to configure automatically.
- TODO: Figure out special purpose Emacs configuration.
- - paredit, Shift-modifiers, etc.
- TODO: Figure out the numpad on the LK400-series keyboard.


# TROUBLESHOOTING

The one hiccup I ran into following online tutorials was adding and
using an entry from `/etc/gettytab`. I discovered from the
`system.log` that the process was exiting with failure upon trying to
launch `getty`. This is why, above, I've opted to just use a standard
entry from `gettytab` instead of my own.


# THANKS & RESOURCES

Thanks to Mark Skilbeck and Toby Thain for the help.

These websites were useful:

- http://www.club.cc.cmu.edu/~mdille3/doc/mac_osx_serial_console.html
- http://archive.download.redhat.com/pub/redhat/linux/7.0/en/doc/HOWTOS/Text-Terminal-HOWTO
- https://www.tldp.org/HOWTO/Serial-HOWTO-19.html
- https://www.tldp.org/HOWTO/Modem-HOWTO-7.html
- http://manx-docs.org/collections/mds-199909/cd3/term/vt420rm2.pdf
  - https://vt100.net/docs/vt420-uu/chapter5.html#S5.3
- http://www.ftdichip.com/Support/Knowledgebase/index.html?an232b_04flowctl.htm
- https://superuser.com/questions/345005/how-to-do-hardware-dtr-dsr-flow-control-on-linux-serial-port-programming
- https://stackoverflow.com/questions/957337/what-is-the-difference-between-dtr-dsr-and-rts-cts-flow-control
- http://comp.terminals.narkive.com/2d3W75ua/dec-vt420-no-hardware-flow-control
- https://www.freebsd.org/cgi/man.cgi?gettytab
- https://lists.gnu.org/archive/html/emacs-devel/2004-09/msg00048.html
- https://gist.github.com/albertfilice/0f12dc87f8d1ec02ef14
- https://superuser.com/questions/1059744/serial-console-login-on-osx
