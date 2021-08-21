---
layout: post
title: Caps Lock as Escape and Control revisited
author: Zubair Abid
categories: [Configuration]
tags: [keyboards, setup, ergonomics, configuration]
---

As I've said [before](./escapectrl),
my Caps Lock key serves as both my Left Control
key (if I hold it), and my Escape key (if I tap it).

This configuration has a bug: occasionally entering `Esc` when I want an empty
`Left Ctrl`. Essentially, when I hold down on the key and then let go without
tapping any other key, the expected behaviour is
that it should behave like tapping a `Left Ctrl`. The actual behaviour is that
of tapping `Esc`, which is not the greatest key to accidentally tap.

# Using Interception Tools' Dual function keys

I've talked about the plugin in [the post about trying to setup home row
mods](./escapectrl).

Install [interception-tools] and [dual function keys]. The installation process
is available at the links; for Arch systems you can run
`sudo pacman -Syu interception-tools interception-dual-function-keys`.

Then, configuration. Setup the following job
`/etc/interception/udevmon.yaml`:

```yaml
- JOB: "intercept -g $DEVNODE | dual-function-keys -c path_to_config.yaml | uinput -d $DEVNODE"
  DEVICE:
    EVENTS:
      EV_KEY: [KEY_CAPSLOCK, KEY_ESC]
```

The config can be setup wherever is appropriate.

```yaml
TIMING:
    - TAP_MILLISEC: 200
    - DOUBLE_TAP_MILLISEC: 0
MAPPINGS:
    - KEY: KEY_CAPSLOCK
      TAP: KEY_ESC
      HOLD: KEY_LEFTCTRL
    - KEY: KEY_ESC
      TAP: KEY_ESC
      HOLD: KEY_CAPSLOCK
```

## Caveat with multiple configs

Generally, caveats as mentioned in the [home row mods
post](./homerow_interception_1) apply. The key details
are to ensure that

1. The udev file should be stored in `/etc/interception/udevmon.yaml`
1. Not more than one config can be used. If you want other key replacements like
  home row mods, the udev will remain as is and the yaml config will be:

```yaml
TIMING:
  ...
MAPPINGS:
  # Enter your other mappings
  - KEY: KEY_CAPSLOCK
    ...
```

[interception-tools]: https://gitlab.com/interception/linux/tools
[dual function keys]: https://gitlab.com/interception/linux/plugins/dual-function-keys/
