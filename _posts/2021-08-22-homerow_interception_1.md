---
layout: post
title: Attempting to setup Home Row mods with Interception tools -- Part 1
author: Zubair Abid
categories: [Configuration]
tags: [linux, emacs pinky, keyboards, setup, ergonomics, configuration]
---

This is a log of sorts of the first time I tried to setup home row mods on my
computer using [interception-tools]. The attempt was unsuccessful, due to
the stock [dual function keys] plugin not being optimised for the mod.

The first two sections are essentially information (fluff?) on what home 
row mods are, so
skip to [process](#process) for the main content.

- Contents
{:toc}

# Context

I want to be comfortable using my keyboard to navigate about my computer.
So far:
i3, vim keybindings everywhere that is sane, relearning how to type from
scratch, and [a little script that converts](./capsesc_dual)
my Caps Lock key to a combined Escape and Control have helped. I've also ordered
[an ergo split keyboard] but we do not talk of that yet.

[an ergo split keyboard]: https://github.com/TweetyDaBird/Lotus58

Currently, my CapsLock as Control/Escape is
achieved using [interception-tools], which allows me to take advantage of
the feature even when the X Server is not running, such as when using a TTY, or
if I were to setup Wayland. Interception tools' Gitlab contains 
[several plugins],
including [the one I have earlier used](./escapectrl) -- caps2esc --
and another one that caught my eye called "[dual function keys]".

[several plugins]: https://gitlab.com/interception/linux/plugins

Incidentally, [I have since replaced caps2esc entirely with dual function
keys](./capsesc_dual).

# So what are home row mods? 

[This article explains it best](https://precondition.github.io/home-row-mods),
but to summarise: `Ctrl`, `Shift`, `Alt`, and `Super` (`Sys/Gui/Winkey`),
referred to henceforth as "modifiers", are brought up to
the home row (the row with `asdf` and `jkl;` that your fingers rest on while
typing). More specifically, the aforementioned keys: `a`, `s`, `d`, `f`, and 
`j`, `k`, `l`, `;`, are also given "secondary functions" when held down instead
of just pressed. So "tapping" `a` (pressing it down for less than a specified
time, often 200ms or so to provide an example) registers on the computer as 'a',
but holding it returns `Alt` (or any other key -- how you configure it is up to
you). Accordingly, the four modifiers are assigned some combination of `a`, `s`,
`d`, and `f`. This is mirrored to the other side on `j`, `k`, `l`, `;` to ensure
that all keys are available at all times.
It has (in my limited testing) significant ergonomic
benefits. However, this configuration is usually done using keyboard firmware
known as QMK, which does not run on many cheap mass-produced boards -- like the
ones I have (for now).

Which is where the plugin called "[dual function keys]" catches the eye.
Theoretically, we should be able to use it to setup dual-functionality on the
necessary keys on any Linux machine independent of the display server in use.
And that is true -- but there are quirks. This is an attempt at documenting and
logging those quirks, and my attempts at overcoming them.

# Process

## Installation 

Install [interception-tools] and [dual function keys]. 
Instructions are on the README; if you're on arch then 
`sudo pacman -Syu interception-tools interception-dual-function-keys`
will work.

## Configuration

Fundamentally, all one needs to do is add this job in
`/etc/interception/udevmon.yaml`:

```yaml
- JOB: "intercept -g $DEVNODE | dual-function-keys -c path_to_config.yml | uinput -d $DEVNODE"
  DEVICE:
    EVENTS:
      EV_KEY: [<keys_being_used>]
```

**Note:** I am not sure how important the `EV_KEY` is, but I don't need to fill
it in to make it work...

Then, the next step would be to write the config file itself, which would follow
the broad formulae of:

```yaml
TIMING:
    - TAP_MILLISEC: 200
    - DOUBLE_TAP_MILLISEC: 0
MAPPINGS:
    - KEY: KEY_A
      TAP: KEY_A
      HOLD: KEY_LEFTALT
    - KEY: KEY_S
      TAP: KEY_S
      HOLD: KEY_LEFTMETA
    - KEY: KEY_D
      TAP: KEY_D
      HOLD: KEY_LEFTSHIFT
    - KEY: KEY_F
      TAP: KEY_F
      HOLD: KEY_LEFTCTRL
    - KEY: KEY_J
      TAP: KEY_J
      HOLD: KEY_RIGHTCTRL
    - KEY: KEY_K
      TAP: KEY_K
      HOLD: KEY_RIGHTSHIFT
    - KEY: KEY_L
      TAP: KEY_L
      HOLD: KEY_RIGHTMETA
    - KEY: KEY_SEMICOLON
      TAP: KEY_SEMICOLON
      HOLD: KEY_RIGHTALT
    - KEY: KEY_CAPSLOCK
```

There are some quirks here I want to discuss first.

- **The location of the udev rule:** 
  This is possibly a `udevmon` thing that I need to get familiar with, but what
  I know as of now:

  So I have on previous occasions used
  `/etc/udevmon.yaml` to configure interception. The documentation specifies
  both `/etc/interception/udevmon.yaml` and a variety of subfolders such as 
  `/etc/interception/dual-function-keys/<name>.yaml` and others. There is
  also a `/etc/interception/udevmon.d/` in which the files can possibly go.

  However, **nothing but `/etc/interception/udevmon.yaml` worked**,
  for some reason.
- **A single config file needs to contain all your configurations**: 

  This one messed with me for a bit. But you can't have two rules in
  `/etc/interception/udevmon.yaml` like

    ```yaml
    - JOB: ... homerowmods.yaml ...
    - JOB: ... caps2esc.yaml ...
    ```

  Only the first job will run.

  If you want to have both home row mods and caps/esc replacement, then all your
  configurations have to be under the same config file. This again, is possibly
  a udev thing I'm not too familiar with.

Also, I want to point out a stupid mistake I made that I hope you will not: I
(initially) named my config file "config.**yml**", while the reference to the
file in udev was "config.**yaml**". Please don't do this.
  

# Issue

Performance is suboptimal. Without the "Ignore Mod Tap Interrupt"
configuration option (among others) specified in the
[aforementioned
article](https://precondition.github.io/home-row-mods#tap-hold-configuration-settings),
it is very easy to

1. mistype a bunch, eg: typing "ater" instead of "father".
2. accidentally activate window manager functions. With my setup, I couldn't
   type "le", as it was read as `Super` + `e` instead, which changes a layout
   option in i3.

The issue is simple: when typing quickly, it is common to "slide" across letters
-- start typing the next letter while the previous one is still pressed down.
Essentially,

Current:

```
                <-- <200ms -->
                         <-- <200ms -->
keyboard:       l↓       e↓   l↑       e↑
computer sees:  Super↓   e↓   Super↑   e↑
```

This is suboptimal, and under ideal conditions, the behaviour would be similar
to:

Desired:

```
                <------ <200ms ------>
                       <------- <200ms ------->
keyboard:       l↓     e↓             l↑       e↑
computer sees:  Super↓ Super↑ l↓ e↓   l↑       e↑
```

Or better yet (maybe),
(Better) Desired:

```
                <---- <200ms ---->
                       <----- <200ms ----->
keyboard:       l↓     e↓         l↑       e↑
computer sees:         l↓ e↓      l↑       e↑
```

# Solution

Unfortunately, configuring the plugin for home-row mods is not very
straightforward -- one can check the [open issue] on the matter. The plan
currently is to spend the next post exploring how an optimal mod-tap would work,
along with comparisons with a QMK keyboard (a Soyuz I built a couple of days
back that will suffice for experimentation).


[open issue]: https://gitlab.com/interception/linux/plugins/dual-function-keys/-/issues/6
[interception-tools]: https://gitlab.com/interception/linux/tools
[dual function keys]: https://gitlab.com/interception/linux/plugins/dual-function-keys/
