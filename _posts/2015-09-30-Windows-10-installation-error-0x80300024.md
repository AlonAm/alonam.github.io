---
layout: post
title: Windows 10 installation error 0x80300024
category: blog
published: false
----
While installing Windows 10...
Apparently this is a known error issue.
When installing windows on a multiboot system you can sometimes get this rather cryptic error message.
It relates to how Windows setup sees your Drive partitions.

To fix it

- Disconnect all drives from your PC except the one you are installing Windows on.
- Enter your BIOS and set the hard drive you are installing Windows on to be the top-priority drive.

