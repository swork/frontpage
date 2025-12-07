---
title: Proxmox 9
---
A long time ago I set up a Proxmox host on an unused Dell laptop. I'd learned of it at [MakeHaven](https://www.makehaven.org/), the fabulous maker-space workshop in New Haven CT, ask me about it sometime - JR the ED asked if I knew anything about it (a free-software world alternative to VMWare ESX) and I had to say No.

I didn't know what I'd be doing with it, so the machine never got physically installed anywhere safe or permanent. It's been leaning on its edge with cords coming out against my file cabinet for at least two years, closer now to three. In that time it did grow some purpose:

 - a CouchDB instance under lxc, serving as collection point for some data acquisition projects 
 
 - a Nextcloud instance, which itself hasn't been used for much beyond giving me some familiarity with Nextcloud
 
 - a Wireguard endpoint for an intention to VPN into AWS, again not fully realized
 
 - various temporary VMs either supporting specific projects or just giving me a chance to play with something system-oriented without dedicating hardware to it
 
 - most recently, a QEMU instance running the Unifi server that monitors our wifi access points.
 
 In that time it also grew some serious oldness. Like, warning banners splashed across the top of all pages saying that Proxmox 7 will go out of support something like two years ago.
 
The path to upgrades for Proxmox are various, but they all end with an admonition that the best choice is to install anew on a different hardware platform and then repave the one you have, migrating VMs and containers via the backup mechanism.

## A new Proxmox install

Okay, cool. I have leaning against the other side of that file cabinet an unused Macbook Pro. Up comes the PVE9 installer, it boots and appears happy, can't connect but I'll figure it out later. (PVE doesn't like wifi, good, but the MBP was of the Thunderware era - Thunderwire? - that required dongles for Ethernet; I plugged it in, saw an interface with the address I'd given the installer, and figured we were good.)

That was three weeks ago, and today came the "figure it out" part. I made a VM backup from the PVE7 installation, and tried to move it to the PVE9 host. Turns out that interface was "vmbr0", which `brctl` told me included no NICs, because there are no NICs: My Thunderbolt dongle requires explicit authorization before the hardware will let it talk to the system. I mean, good for Apple to [undermine an overused trope](https://en.wikipedia.org/wiki/No_Time_to_Die). And it wasn't that bad to figure out how to steal libpolkit-gobject and bolt packages from elsewhere and transfer them in via USB stick (that trope again! But this machine does have a USB port.)

The whole `boltd`/`boltctl` thing is fun. Thunderbolt gets its mojo by allowing external devices to DMA straight to system RAM, without involving the CPU - this is what I get from [the website](https://gitlab.freedesktop.org/bolt/bolt). So putting in place a hardware authorization mechanism seems like a good idea.

$20 has a couple more dongles on the way from Ebay. The plan is to reinstall a new PVE9 over the old one, then cluster the two machines together with (as they recommend) dedicated NICs for inter-node activity. The two will go side by side in a rack at the back of a shelf in the garage, on edge hoping for a bit more convective cooling. And with a bunch of gaffer's tape holding the dumb magnetic power attachment to the MBP, which maybe represents the worst mis-feature of this machine deployed as a server. But it's what I have.
