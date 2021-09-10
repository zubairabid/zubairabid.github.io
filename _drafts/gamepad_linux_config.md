---
title: Configuring the Logitech F710 Controller on Arch
author: Zubair Abid
categories: [gaming, linux, configuration]
tags: [configuration]
---

It's supposed to be seamless on Linux TODO link,
but my case seems to be an exception.

# Rough points

- Works perfectly on my laptop
- Enabling vibration kills the process for a few seconds whenever a vibration
  signal is sent.
- There is a `js0` and a `js1`. Can send vibration to former and nothing else,
  can send everything else to latter but not vibration (earlier bug). Used
  <https://gamepad-tester.com/>. In XInput mode. In DInput mode, doesn't vibrate
  on `js0` either.
- Can use `fftest` on the one that says `event` on it.
- On evtest, device 257 works, not device 29.
