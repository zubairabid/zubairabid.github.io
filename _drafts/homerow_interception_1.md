---
title: Attempting to setup Home Row mods with Interception tools -- Part 1
author: Zubair Abid
categories: [Configuration]
tags: [keyboards, setup, ergonomics, configuration]
---

# Context

I want to be comfortable using my keyboard to navigate about my computer. So far
i3, vim keybindings everywhere that is sane, relearning how to type from
scratch, and a little script that converts
my Caps Lock key to a combined Escape and Control have helped. I've also ordered
an ergo split keyboard but we do not talk of that yet.

The crucial bit here is the 'script' converting my Caps Lock to Control/Escape.
This is achieved using interception tools, which allows me to take advantage of
the feature even when the X Server is not running, such as when using a TTY, or
if I were to setup Wayland. Interception tools' Gitlab contains several plugins,
including the one I used -- caps2esc -- and another one that caught my eye
called "dual-function-keys".

# So what are home row mods? 

[This article explains it best](https://precondition.github.io/home-row-mods),
but to summarise: `Ctrl`, `Shift`, `Alt`, and `Super` (`Meta/Winkey`),
referred to henceforth as "modifiers", are brought up to
the home row (the row with `asdf` and `jkl;` that your fingers rest on while
typing). More specifically, the aforementioned keys: `a`, `s`, `d`, `f`, and 
`j`, `k`, `l`, `;`, are also given "secondary functions" when held down instead
of just pressed. So "tapping" `a` (pressing it down for less than a specified
time, often 200ms or so to provide an example) registers on the computer as 'a',
but holding it returns `Alt` (or any other key -- how you configure it is up to
you). Accordingly, the four modifiers are assigned some combination of `a`, `s`,
`d`, and `f`. This is mirrored to the other side on `j`, `k`, `l`, `;` to ensure
that all keys are available to enter (if you wanted to enter `Shift+d` and the
only key for `Shift` was hold+`d`, that would only be possible if another key
was also able to shift). It has (in my limited testing) significant ergonomic
benefits. However, this configuration is usually done using keyboard firmware
known as QMK, which does not run on most cheap mass-produced boards -- like the
ones I have (for now).

Which is where the package called "dual-function keys" catches the eye.
Theoretically, we should be able to use it to setup dual-functionality on the
necessary keys on any Linux machine independent of the display server in use.
And that is true -- but there are quirks. This is an attempt at documenting and
logging those quirks, and my attempts at overcoming them.
