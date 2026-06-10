---
title: Configuring the ai03 Soyuz as a Macropad using QMK on Linux
author: Zubair Abid
categories: [Configuration]
tags: [keyboards, build log, setup, configuration, qmk]
---

For the build log of the Soyuz, check [the previous post](TODO Link). This post
documents

- The setup process for QMK, along with quirks to keep in mind
- How to flash the microcontroller with the firmware
- My configuration at the time of writing

This post **does not use QMK Configurator**. Please check the QMK docs for
better resources there. It also focuses entirely on my personal OS setup, 
although any Linux variant should be largely similar.

# QMK and firmware

For a general build guide, the [QMK
Docs](https://docs.qmk.fm/#/newbs_getting_started) might be better suited to the
task. This post should cover most crucial content, however.

## Installing QMK

First, setting up the prerequisites: we'll need `git`, `python` (which should be
pre-installed), `python-pip`, `libffi`. On top of this, I also use Python
virtualenvs (which might be affecting some functionality).

```bash
sudo pacman -Syu git python-pip libffi
```

- install qmk pip
- fork and clone repo
- run setup
  - install if asked
  - get submodules


### Quirk with sudo

## Flashing the numpad

# Configuration

- Hmmm

