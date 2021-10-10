---
layout: post
title: Figuring out virtualization on Linux - Part 1, Basics, Setting up a VM.
author: Zubair Abid
categories: [Computing]
tags: [discovery, linux, kvm, qemu, libvirt, os]
---

Not so much a guide as it is a learning-log of sorts.

# Figuring out basics of virtualization

The goal is to understand at least a little bit of what I do.

Things I am aware of before starting this trip:

1. Virtualization can be achieved with `VMWare`, `Virtualbox` 
   (as I have used in the
   past), but solutions like `Hyper-V` and `KVM`
   on Windows and Linux respectively
   offer much more in terms of hardware (CPU) acceleration.
1. I want to set it up for experimentation purposes, because computers are cool
   and networking seems cool and I want to explore more of both. I'm also really
   fascinated by DevOps and realistically will have to interact with it often
   enough, and want to know what it is instead of pressing random buttons
   prescribed by obscure internal documentation.
1. There's more than just `KVM` available, but it seems like a well-supported,
   popular option.
1. If I want to use `KVM`, I will probably want to use `QEMU`, `libvirtd`,
   `virt-manager`?, `virsh`?, and ???.

Now the [Arch Wiki page](https://wiki.archlinux.org/title/QEMU) is rich in
detail but not in explanation, so that won't serve as a good starting point.
Thankfully, I found [octetz' intro-level guide to virtualization with KVM] and
Veronica Cary's [QEMU/KVM for absolute beginners]. These videos provide both a
high-level overview of what is happening amongst all the moving parts, and a
step-by-step guide to setting up the tools.

At this point, I know:

1. `KVM` is the hypervisor we will be using. It's baked into the kernel.
1. `QEMU` is the actual virtual machine emulator. 
   It emulates a bunch of things and
   interacts with `KVM` (although it can work without it too, slower).

   I am not aware of how it's handling emulation, exactly. Storing the
   documentation
   [here](https://qemu-project.gitlab.io/qemu/system/device-emulation.html), for
   future reference.
1. `libvirtd` is a generic virtualization API that works on top of several
   technologies, that we will use for working with `KVM`/`QEMU`.
1. `virt-manager`/`virsh` are front-end applications that interact with the 
   `libvirt`
   API when required. `virt-manager` also comes with `virt-install`
   and `virt-viewer`.
1. It seems that for networking, `bridge-utils` is used. I am not too clear on
   networking yet, however.
1. `libvirtd` has a concept of "system" and "session" instances. As a quick
   summary: in system mode, `libvirtd` is running as root and has more access to
   things in general. For a less hacky explanation, [this post seems
   appropriate](https://blog.wikichoon.com/2016/01/qemusystem-vs-qemusession.html).
   - On this note, `virt-manager` uses system mode by default, but `virsh` uses
     user mode. **We will need to configure `virsh` to use system mode by
     default**,
     as that's what we're using to begin with. Why? Because that's what the
     videos I saw said would be fine most of the time. I'll make my own
     decisions once I'm more aware. The tradeoff seems to be worse
     desktop-use-case integration for better networking (with system).

Alright, it's now time to set it up.

# Setting up `virt-manager` and `virsh` to work with `libvirtd`, `KVM`, and `QEMU`

## Ensure KVM will work

First, we need to ensure that KVM is set up and ready. I ran this command from
the Arch wiki:

```sh
$ LC_ALL=C lscpu | grep Virtualization
Virtualization:                  AMD-V
```

I have an AMD CPU, and this confirms that KVM is ready.

## Installing everything else

Again, I'm not sure _exactly_ what is needed for the task, but this should
suffice. I'm using pacman, adjust as necessary.

```sh
$ sudo pacman -Syu qemu qemu-arch-extra libvirt lxc arch-install-scripts
iptables-nft dnsmasq bridge-utils openbsd-netcat virt-viewer virt-manager
virt-install dmidecode
```

Things I am sure we need: `qemu`, `libvirt`, `lxc`, `bridge-utils`,
`virt-manager`. I do not know about the rest.

## Setting up permissions

From what I can tell, this has primarily two parts:

1. **Configuring `virsh` to work with `qemu:///system` by default**

   ```sh
   sudo cp -rv /etc/libvirt/libvirt.conf ~/.config/libvirt/
   sudo chown zubair:wheel ~/.config/libvirt/libvirt.conf
   ```
  
   Then, edit the file and uncomment the last line to say: `uri_default =
   "qemu:///system"`
1. **Ensuring that the user has permissions to interact with `libvirtd`**
  
   Here, [the octetz blog I'm
   using](https://octetz.com/docs/2020/2020-05-06-linux-hypervisor-setup/#permissions)
   specifies three ways, mentioning that if the user is in an administrator
   group (like `wheel` in arch), they probably won't have to worry. I'm _really_
   not clear though -- the wiki mentions that a password prompt is necessary,
   but the video/post does not allude to it. I have decided to take the "safe"
   option that is mentioned and add my user to the `libvirt` group.

   ```sh
   sudo gpasswd -a zubair libvirt
   ```


[octetz' intro-level guide to virtualization with KVM]: https://octetz.com/docs/2020/2020-05-06-linux-hypervisor-setup/
[QEMU/KVM for absolute beginners]: https://www.youtube.com/watch?v=BgZHbCDFODk
