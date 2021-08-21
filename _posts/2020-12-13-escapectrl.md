---
layout: post
title: Using Caps Lock as both Escape and Control
author: Zubair Abid
categories: [Configuration]
tags: [linux, ergonomics, emacs pinky, configuration, setup] 
---

**Update, 2021-08-21:** I currently use `dual function keys` instead. Check
[this post](./capsesc_dual)

My keyboard is configured (at the OS software level) to recognise a tap of the
Caps Lock key as Escape, and holding it down as Control. Why? Ergonomic
benefits, RSI, etc. It isn't perfect, but better than the default for specific
use cases (that I meet).

I used [Danny Guo's blog], more specifically [caps2esc]. This post is to note
some specifics/quirks.

- This configuration works on any keyboard, even externally attached ones. I
  consider this a Good Thing.
- The keyboard will not light up the Caps Lock indicator if you press Caps
  Lock, as it is now Control/Escape. *However*, some keyboards will light up the
  indicator if you press Escape (actually Caps Lock), but this is dependent on
  the keyboard. My Ducky did this correctly, but the Thinkpad keyboard did not.
  Your experience might differ on this specific.
- Also, as the configuration is at the software level, the keyboard itself is
  unchanged. So if you need Escape to configure stuff on your keyboard -- like I
  do with my Ducky -- then it works flawlessly. In my use cases so far, anyway.
- This has to be configured per-OS installed, per-laptop. This is also
  considered a Good Thing by me.
- If you have Windows installed this will not work on Windows. Or any other OS
  installed on the system. It's obvious, but worth mentioning. For my Windows
  install, I used the same blog [to configure AutoHotkey]
- The `Left Control` remains as a Control key. This is useful because,
- Sometimes in games and other situations holding down Caps is not recognised as
  pressing Control [^loss]. This is when the Left Control still functioning as a
  Control is Very Useful.

There are a few general quirks with respect to getting used to it, but it's a
solid mod otherwise, well worth doing.

[Danny Guo's blog]: https://www.dannyguo.com/blog/remap-caps-lock-to-escape-and-control/
[caps2esc]: https://gitlab.com/interception/linux/plugins/caps2esc
[to configure AutoHotkey]: https://www.dannyguo.com/blog/remap-caps-lock-to-escape-and-control/#windows

[^loss]: An example - pressing Ctrl+Left and expecting to go back one word in Google Docs. Will not work.

