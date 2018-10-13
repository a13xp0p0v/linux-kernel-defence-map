# Linux Kernel Defence Map

## Intro

Linux kernel security is a very complex area. It would be nice to have some
graphical representation of its current state. So I've created a __Linux Kernel
Defence Map__ showing the relations between:
- vulnerability classes / exploitation techniques,
- kernel defences,
- bug detection means.

__N.B.__ The node connections don't mean "full mitigation". These connections
represent some kind of relation. So ideally, this map should help to navigate
in documentation and Linux kernel sources.

I wrote it in DOT language and generated the picture using GraphViz:
```
dot -Tpng linux-kernel-defence-map.dot -o linux-kernel-defence-map.png
```
So it is very pleasant to maintain this map with git.

If you see any mistakes, feel free to create an Issue or ping me via alex.popov@linux.com

## Documentation

- Grsecurity features:
https://grsecurity.net/features.php

- The State of Kernel Self Protection by Kees Cook:
https://outflux.net/slides/2018/lca/kspp.pdf

- Linux kernel security documentation:
https://www.kernel.org/doc/html/latest/security/self-protection.html

- Linux kernel mitigation checklist by Shawn C:
https://github.com/hardenedlinux/grsecurity-101-tutorials/blob/master/kernel_mitigation.md

## The Map for v4.19

![Linux Kernel Defence Map](./linux-kernel-defence-map.png)

