---
layout: post
title: Flashing QMK firmware onto Pro Micros using Linux
author: Zubair Abid
categories: [Keyboards]
tags: [Keyboards, configuration, linux, pro-micro]
---

You need to run:

```bash
sudo make <keyboard_folder>:<keymap-folder>:avrdude
sudo make ai03/soyuz:soyuz_macro:avrdude
```

Instead of `sudo qmk flash -kb ai03/soyuz -km soyuz_macro`.

(Also remember to flash `RST` and `GND`.

Not entirely sure why. Might have to do with Arduino specifics. Not an issue
with my Elite-C (v4).
