# Installing Caelestia on ArchLinux with archinstall

Look, I get it. Arch isn't easy. But then, it's not intended to be easy. All
Linux distros are just a collection of software built on the Linux kernel, but
some are definitely easier for a beginner to understand than others. Distros
like Ubuntu and Fedora include a ton of custom software to make installing,
configuring, and working with the system easier. Arch doesn't include anything
of the sort. Instead, Arch take a different approach. In fact, the Arch
can be viewed as an extension of the Linux philosophy: do one thing, and do
it well. Rather than trying to hold your hand through the process, Arch expects
you to learn as you go. Sure, it's harder to get started. But once you have a
working Arch install, you've built something that's uniquely tailored for your
needs, hardware, and tastes; not something that's modified to fit someone
else's definition of the perfect system.

Recently, we've seen a significant increase in new users trying to install
Caelestia. And I don't mean users new to Caelestia, I mean users who have
never used Linux and are trying to figure out how to install Arch so they
can have Caelestia. This guide is an attempt at providing simple, easy to
follow instructions for doing exactly that. It won't make you a power user,
and it won't teach you everything you need to know, but it *will* help you
get through an initial Arch install... so you can get to breaking things
and learning how to use Linux properly faster.

## A Note on Safety

Our [Discord server] includes a warning in the #resources channel that I'm
going to repeat here: If you don't know what a command does, as a general rule
don't run it! We encourage our users to check out what a command they aren't
familiar with does before actually running it. [explainshell] is a great
resource for this.

## Getting Started

Before you can install Arch, you'll need an install image! Chances are, you
probably already have this and may even have written it to a USB drive of some
sort. On the off chance you haven't, the [Arch install image] is available on
their website.

It's also recommended that users follow the instructions on the download page
in the **Download verification** section. Verification ensures that both your
image downloaded without corruption, and that it was distributed from a known
authentic source. If either the `b2sum` or the signature fails, redownload from
a different mirror!

If you don't know how to write an ISO image to a USB drive, the Arch wiki
provides a thorough resource on the [USB flash installation medium] page which
covers creating a bootable USB drive on Linux, Windows, and Mac.

### How to use this Guide

This guide will technically walk you through the process of installing both
Arch and Caelestia, but you won't learn much by just following the guide. It is
highly recommended that you use this as a reference, in conjunction with the
official [Arch Installation guide]. The official guide will go into *much* more
detail than this guide will, and you're more likely to learn something from it.

## Starting the Installation

Once you have booted up your live image, you will be presented by a simple
terminal welcoming you to the Arch installation medium, and suggesting that
you verify your internet connection and follow the [Installation guide].
Verify you have internet access by running `ping ping.archlinux.org`.
If it times out, you will need to manually setup your internet connection. To
ensure your network interface is listed and enabled, run `ip link`. For
wireless and WWAN, make sure the card is not blocked with [rfkill].

You have 3 main options to connect to the network:

- **Ethernet**: plug in the cable.
- **Wireless connection**: authenticate to the wireless network using [iwctl].
- **Mobile broadband modem**:connect to the mobile network with the [mmcli] utility.

Once you are connected to the internet, run `archinstall`.

## Configuring your Installation

The Arch install script provides a simple TUI that effectively walks you
through the install process. Most options are up to you, but we're going
to choose a few specific things during the install process, so follow along;
we'll tell you when you need to pick something specific.

### Regional Configuration

The first few steps in the process allow you to set your language and locale,
and choose specific mirrors for your downloads. The default language and
locale are US English, but you are welcome to change these to suit your
geographic situation. Mirror selection is less important during the
installation, as the installer itself runs [Reflector], a tool that optimizes
the mirrorlist on demand.

### Disk Configuration

The next step is setting up your hard drive(s). The installer will offer to
create a best-effort default partition layout, and this may well be sufficient
for many users. Choosing a filesystem is also mostly up to you, though for new
users the most commonly recommended filesystem types are *btrfs* and *ext4*.
If you don't know what to choose, *ext4* is the default for many distros, and
is a good choice for your first install. In the next step, swap should be
enabled by default.

### Bootloader and Kernel Configuration

The Arch installer usually defaults to using GRUB for its bootloader. If it
doesn't, it's recommended that you change it to GRUB unless you know what you
are doing or have a specific need not met by GRUB. Similarly, the default
*linux* kernel is fine for most users.

### Hostname and Authentication

Choosing a hostname is entirely up to you. Your choice will have no bearing on
anything beyond what your computer calls itself, and what other computers on a
local network will see it as.

In the Authentication section, you'll want to do a few things. First off, set a
good password for the *root* account. Second, we're going to add a new user
account because you shouldn't be using the root account directly for much of
anything. In fact, Wayland will complain if you try to launch a session as
root. Set the username and password to whatever makes you happy, then select
the option to make your new user a superuser. This will allow your user to run
administrative commands.

### Profile Selection, Applications, and Networking

Caelestia is built on Hyprland, so you want the Hyprland profile, right? Wrong!
The Hyprland profile bundles its own opinionated things and doesn't *quite*
line up with our needs. Instead, choose the *Minimal* profile.

Under the Applications section, enable bluetooth if your computer supports it,
and set audio to *pipewire*. Under Network Config, we're going to choose
*Use Network Manager (default backend)*.

While the *Minimal* profile gives us a basic Arch install, there are a few
other packages you'll need in order to get Caelestia working, so we're going to
add them in the Additional Packages section. Add the following packages before
moving on: `base-devel` and `git`. Once all are selected, press *enter* to
return to `archinstall`.

Finally, set your timezone to whatever is geographically accurate for you, and
hit the Install button!

## Rebooting into Arch

Once the installation is finished, you will be presented with a prompt asking
if you want to reboot. You do. When prompted, remove the installation medium
and when you reboot you will find yourself at a login prompt. Login as the
user you created during the install, and once again ensure you have internet
access.

Keep in mind that you won't have an active internet connection right after
rebooting. You can easily connect using nmtui, a text-based interface
for NetworkManager. It's intuitive enough that you won't need a guide to
figure it out.

## Installing Caelestia

Caelestia is generally fairly straightforward to install, though if any of the
subsequent steps cause issues please feel free to let @Evertiro know on our
[Discord server].

As many of the packages we rely on aren't in the official Arch repos, the first
thing you'll want is an AUR helper. It's possible to install the dependencies
manually, but having a helper will make your life much easier. The most
commonly used AUR helpers are `paru` and `yay`. Pick your preferred helper,
and follow the below instructions to install it:

### paru

```bash
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```

### yay

```bash
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

Once you have installed your preferred AUR helper, it's time to install the
Caelestia CLI app. For the remainder of this guide, if you installed `yay`,
simply replace `paru` with `yay` in the following command. When asked whether
you want to install `caelestia-cli` or `caelestia-cli-git`, select the standard
(non-git) version.

```bash
paru -S caelestia-cli
```

Once the install is completed, double-check that it installed successfully by
running `caelestia -v`. It should output the installed version information for
Caelestia and Quickshell (most of which will show as not installed).

Assuming all went well and you have a working `caelestia` install, it's time
to install the rest of the environment! Run `caelestia install` to get started!
Once the installer starts, you will be prompted to select which components you
want to install. The choice is yours, and you can always adjust later, so pick
whatever you want.

Installation will take a while, but assuming you have no errors, when the
installer finishes you should reboot. Once you have logged in again, you can
launch your new Caelestia install simply by running `start-hyprland`.
Congratulations!

Obviously, this is far from a complete system; a display manager would be nice,
and there are tons of other "necessary" apps that you'll want to install.
However, much of that is subjective, so not covered by this guide. If you are
now looking at a pretty Caelestia install, you're at a point where we can
more easily help you figure out the rest on Discord. If you ran into an error,
please ping @Evertiro on our [Discord server] and he'll help you figure out
how to work through it and improve this guide for the next person!

**Welcome to Caelestia!**

[explainshell]: https://explainshell.com/
[Discord server]: https://discord.gg/BGDCFCmMBk
[Arch install image]: https://archlinux.org/download/
[USB flash installation medium]: https://wiki.archlinux.org/title/USB_flash_installation_medium
[Arch Installation guide]: https://wiki.archlinux.org/title/Installation_guide
[Installation guide]: https://wiki.archlinux.org/title/Installation_guide
[rfkill]: https://wiki.archlinux.org/title/Rfkill
[iwctl]: https://wiki.archlinux.org/title/Iwctl
[mmcli]: https://wiki.archlinux.org/title/mmcli
[Reflector]: https://wiki.archlinux.org/title/Reflector
