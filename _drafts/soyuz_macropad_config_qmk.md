---
title: Configuring the ai03 Soyuz as a macropad with QMK and Linux
author: Zubair Abid
categories: [Configuration]
tags: [keyboards, build log, setup, configuration, qmk]
---

So I got a couple of B-Stock ai03 Soyuz PCBs from StacksKB TODO Link all
for soldering practice, and about three months later I'm in the mood to make it
a functional macropad.

This is not the proper build guide, I'm not using stabilisers for example. For 
that [I'd recommend the build guide on ai03's
github](https://github.com/ai03-2725/Soyuz/blob/master/BuildGuide-English.md).
Although there are some oversights I'll be covering in this log with respect to
microcontroller and diode soldering.

Since this is my first use of QMK, this will also log my initial QMK setup.

- Contents
{:toc}

# Components used in the build

- 1 $\times$ ai03 Soyuz PCB (came with break-off top and bottom FR4 plates)
- 20 $\times$ IN4148 through-hole diodes
- 4 $\times$ 14mm M2 spacers (I recommend getting 16-20mm if you socket the
  microcontroller)
- 8 $\times$ 5mm M2 screws
- ~~40 $\times$ Millmax 0305 sockets~~
- Machined IC Socket (round holes)
- 1 $\times$ Elite C v4
- 20 $\times$ Boba U4T

Technically, I also used

- A pair of tweezers for shorting pins (reset button, basically)
- Foam and tape, for some of the hacks.

# Build

## Soldering diodes

Diodes *can* theoretically be soldered on either side of the PCB, but it's much
more convenient if you solder them on the back -- opposite of where the switches
will be put. If the PCB you got retains the silkscreen art, the backside is the
side opposite it.

![Front of the PCB](/assets/img/soyuz_pcb_front.jpg)

![Back of the PCB, where you should solder the
diodes](/assets/img/soyuz_pcb_back.jpg)

**More importantly, the diodes should be installed such that the polarity is not
reversed.** The PCB silkscreen shows the orientation to use. If messed up in a
consistent fashion, it would be possible to fix it in the firmware, but this is
still just hearsay to me. You'll have to look up how.

![The correct orientation for the diodes](/assets/img/soyuz_diodes.jpg)

Installing them is easy: bend the legs to fit them, solder them in, and then
clip off the diode legs. **Keep the diode legs**. They will be needed for
socketing the microcontroller.

## Socketing the microcontroller

**Important:** If you are not socketing your microcontroller, install the 
switches first. The microcontroller once installed, blocks two switch holes so
they cannot be soldered in after the fact, unless you desolder the
microcontroller -- and good luck with that.

**Even more important:**
Here, installing on the right side of the board is crucial. *Ensure the
microcontroller/socket is installed on the back of the PCB, where we just
soldered on the diodes.* If you mess this up, it would still be possible to run
the numpad with 18 keys instead of 20 by installing the microcontroller
backwards.

![The correct side to install the
microcontroller](./assets/img/soyuz_mc_right.jpg)

![The incorrect side to install the
microcontroller](./assets/img/soyuz_mc_wrong.jpg)

**Important too, I guess:**
The correct way to install the microcontroller is with the side with the
components facing away from the PCB. The silkscreen should specify this, but in
case it didn't, the more you know.

![The correct orientation to install the microcontroller (assuming it's on the
correct side)](./assets/img/soyuz_mc_orient.jpg)

Instead of soldering the microcontroller onto the board, I socketed it using
some 28-pin machined sockets I had to clip to size. [This is a good resource for
how to socket your
microcontroller](https://www.40percent.club/p/socketing-pro-micro.html), but
I'll reproduce the basics on here. Socketing is generally a good idea even if
you don't plan on switching out the microcontroller -- if you mess up soldering
while building it, sockets
are far cheaper and more disposable.

1. Put your sockets into the board and solder **one pin**. 

   If it's a single line of sockets, we want it to be aligned
   straight. For that, heat
   the soldered pin up, and adjust the socket so it is perfectly straight at a 
   90° angle TODO credit nicell.

   Confirm that the socket is on the right side of the board before soldering
   the rest of the pins in.
1. Tape over the socket holes to prevent the microcontroller from fusing with
   the socket. Using the diode legs from earlier, poke holes in the tape. It
   should look something like this:

   ![Diode legs on taped-down sockets](./assets/img/soyuz_diodes_socket.jpg)
1. Put the microcontroller in through the diode legs. Solder at the base. Clip
   off the extra legs.

   ![Soldered microcontroller, before and after
   clipping](./assets/img/soyuz_solder.jpg)

### Testing the PCB

Now is a good time to test the PCB. Plug it in, [flash the controller with the
basic .hex file to start](#) TODO Link, and test all the switch slots by
shorting them with tweezers.

![Shorting to test a switch](./assets/img/gen_short_switch.jpg)

## Mill-max vs soldering: Testing clearance

- Wanted to Mill-max it, and it would have worked, but,
- The 14mm spacers aren't enough for fitting a USB cable in.
- Even without mill-max, with the socketing alone, it's barely a fit

## Hacks/Mods

- Rubber feet? No, foam.
  - Screw bottom scratches the deskmat
  - Obvious solution: use foam and tape it in instead. Not trying to get any
    friction, just desk aesthetics.
- Increasing spacer size:
  - Take foam, fold it up, fit it between top plate and spacers. Very ugly mod,
    but kinda works.

# QMK and firmware

## Setting up QMK

### Quirk with sudo

## Flashing the numpad

# Configuration



- Components used in the build
- Building it:
  - Soldering diodes
  - Socketing the microcontroller
  - Testing Mill-max vs soldering in switches
  - Hacks/Mods to make it fit
- Configuring it:
  - Setting up QMK (in virtualenv)
  - Flashing the numpad:
    - Quirk with sudo
    - Bootloader mode
  - Configuration

