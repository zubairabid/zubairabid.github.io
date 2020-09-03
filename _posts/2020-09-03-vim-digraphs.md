---
layout: post
title: My commonly used vim digraphs
author: Zubair Abid
categories: [Configuration]
tags: [vim, configuration, setup, editing] 
---

I need symbols like ϵ ≠ ± ∩ ∪ from time to time, and have been using 
[vim digraphs](https://vimhelp.org/digraph.txt.html) for the same. This post is
a quick cheatsheet for myself, based on common usage.

**Why not mathjax?** Jekyll (kramdown) has some issues with requiring `$$math$$`
instead of just using `$math$`. Is it an easily solvable problem? Maybe, but 
this is useful anyway because sometimes I need to put math in a non-markdown 
document that will not be rendered. Hence.

# Very basics:

In order to use digraphs, in `Insert` mode, press `Ctrl`+`k`, and then enter
the key combination. This may require multibyte support.

# The list

## Math

| Character      | Digraph            |
|----------------|--------------------|
| ∈ , ∋          | (- , )-            |
| ≤ , ≥ , ≠ , ≡  | =< , >= , != , =3  |
| ⇐ , ⇒          | <= , =>            |
| ₀₋₉            | 0s ... 9s          |
| ⁰⁻⁹            | 0S ... 9s          |
| ⁿ              | nS [^nsnowork]     |
| ∪ , ∩          | )U , (U            |
| ⊂ , ⊃ , ⊆  , ⊇ | (C , )C , (\_, )\_ |
| ≈ , ≃ , ≌ , ≅  | ?2 , ?- , =? , ?=  |

[^nsnowork]: **Note**: `ns` does not work, it returns ش

