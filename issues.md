Export of Github issues for [a13xp0p0v/linux-kernel-defence-map](https://github.com/a13xp0p0v/linux-kernel-defence-map).

# [\#7 Issue](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/7) `closed`: How do I use this?

#### <img src="https://avatars.githubusercontent.com/u/5826484?u=2cc3ddef5824379423495733759ef362d0600078&v=4" width="50">[frakman1](https://github.com/frakman1) opened issue at [2022-10-05 14:41](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/7):

I don't understand the relationship between this project and `kconfig-hardened-check`
I see that you have a `.dot` file that you use a tool (`dot`) to generate the `.svg` diagram from, but how do you generate the `.dot` file in the first place?
Can I use `kconfig-hardened-check` to generate such a diagram? I don't see a way to generate a `.dot` file from `kconfig-hardened-check`.


#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) commented at [2022-10-07 15:28](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/7#issuecomment-1271742617):

Hello @frakman1,

The `.dot` file describing the Map is written manually.

And I created the `kconfig-hardened-check` tool to check some kernel options that you see at the Map.


-------------------------------------------------------------------------------

# [\#6 Issue](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/6) `open`: Try other visualizations of the Map
**Labels**: `enhancement`


#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) opened issue at [2022-07-03 11:00](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/6):

The Map is quite complex.
Let's try other graph visualizations, maybe make it interactive and see how it works.
Example: https://d3-graph-gallery.com/

#### <img src="https://avatars.githubusercontent.com/u/3686800?u=c012685eab3fd216d9337230a4f6e5615f064538&v=4" width="50">[a-andreyev](https://github.com/a-andreyev) commented at [2022-08-24 19:21](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/6#issuecomment-1226143897):

I don't know a ready-to-use solution, unfortunately. I want to share search keywords. I guess it would be helpful.

Talking from an academic point of view, I've heard there is also a need for such tools. Search keywords: _the semantic web_, _ontologies_, _knowledge bases_. (see also: _AI winter_, _knowledge engineering_, _logic programming_). Hard to cover it in a post.

Talking about more practical things, I've heard about ontodia.org tool, which is probably a part of metaphacts project currently (if I'm not mistaken). So, it looks like it also makes sense to search for a regular visualization framework and try to adapt it to the subject area.

Therefore, data format related keywords are probably: _json-ld visualization_, _ttl rdf visualization_, _graphviz visualization_. UI related keywords: _d3js_, _plotly_.

Some related projects I've found:

- https://networkx.org/
- https://github.com/science-periodicals/jsonld-vis
- https://github.com/roholazandie/graph_drawing (https://hilbert-cantor.medium.com/network-plot-with-plotly-and-graphviz-ebd7778073b)
- https://cneben.github.io/QuickQanava/index.html (didn't tried it, but it's in Qt/QML, since I'm a fan :nerd_face: )

Maybe some game engines/sdks also could be helpful, but I have no experience with it.


-------------------------------------------------------------------------------

# [\#5 Issue](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/5) `closed`: Improve the Control Flow Integrity part of the Map
**Labels**: `enhancement`


#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) opened issue at [2021-12-11 22:38](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/5):

The Map should have separate nodes for Forward-edge CFI and Backward-edge CFI.

#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) commented at [2021-12-11 23:14](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/5#issuecomment-991802726):

Introduced the Control Flow Integrity cluster

Changes:
1. Add cluster_cfi with Forward-edge CFI and Backward-edge CFI
2. Rename "CFI_CLANG" to "ARM: CFI_CLANG"
3. Move SHADOW_CALL_STACK to child nodes of cluster_cfi
4. Separate ARM64_PTR_AUTH and ARM64_BTI_KERNEL
5. Describe new connections


-------------------------------------------------------------------------------

# [\#4 PR](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/4) `merged`: Add CONFIG_ZERO_CALL_USED_REGS information (https://git.kernel.org/pub/scm/linux/…

#### <img src="https://avatars.githubusercontent.com/u/86626082?u=d4b762954753e00f72dcaac883bfb7237f7a0c20&v=4" width="50">[MenV1S](https://github.com/MenV1S) opened issue at [2021-10-02 12:52](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/4):

…kernel/git/torvalds/linux.git/commit/security/Kconfig.hardening?id=a82adfd5c7cb4b8bb37ef439aed954f9972bb618)

#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) commented at [2021-10-24 13:31](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/4#issuecomment-950326061):

Hi @MenV1S, thanks for your PR!

Yes, `ZERO_CALL_USED_REGS` is related to `ROP`.
However, it is not related to `Info Exposure`, which is about infoleak between the kernelspace and userspace.
Could you please fix your commit and force-push your branch?

Also, would you like to generate the svg and update it in a separate commit?

Thanks!
Alexander


-------------------------------------------------------------------------------

# [\#3 PR](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/3) `closed`: Link nodes to corresponding CWE records

#### <img src="https://avatars.githubusercontent.com/u/3757038?v=4" width="50">[Self-Perfection](https://github.com/Self-Perfection) opened issue at [2019-06-27 12:31](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/3):

Make nodes in generated SVG clickable when viewing in webbrowser.

This is auto edit made with
`sed -i 's|\([^"]*(CWE-\([0-9]*\))[^"]*"\);$|\1 [URL="https://cwe.mitre.org/data/definitions/\2.html"];|' *dot`
missing some nodes. Mostly as an example to illustrate suggestion to make nodes in SVG linked to something relevant. Uses [URL property](https://www.graphviz.org/doc/info/attrs.html#d:URL).

#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) commented at [2019-06-28 21:48](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/3#issuecomment-506887015):

Hello @Self-Perfection, 
Thanks for the idea.

Yes, this automatic edit both missed some vulnerability classes and added surplus information to the link descriptions.
Would you like to fix it carefully and re-push this again?
(otherwise no problem, I'll fix it myself)

Thanks!

#### <img src="https://avatars.githubusercontent.com/u/3757038?v=4" width="50">[Self-Perfection](https://github.com/Self-Perfection) commented at [2019-06-29 08:47](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/3#issuecomment-506940677):

Please make required adjustments, I don't want to put too much effort in this proposal.

#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) commented at [2019-07-01 10:51](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/3#issuecomment-507215417):

Thanks @Self-Perfection,
I've finished and merged this work.


-------------------------------------------------------------------------------

# [\#2 PR](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/2) `merged`: Add CWE-200 to list of bugs detectable by KMSAN

#### <img src="https://avatars.githubusercontent.com/u/912627?u=882a198d258a9264d8f9114bac8fcc1cab829632&v=4" width="50">[ramosian-glider](https://github.com/ramosian-glider) opened issue at [2018-10-23 12:48](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/2):

To some extent, KMSAN can detect info exposures (aka information leaks). E.g. it reports copying of uninitialized stack/heap data to the userspace. See https://github.com/google/kmsan/wiki/KMSAN-Trophies for examples.

#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) commented at [2018-10-24 08:49](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/2#issuecomment-432570085):

Hello @ramosian-glider,
Thanks a lot for your pull request!


-------------------------------------------------------------------------------

# [\#1 PR](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/1) `merged`: Add address limit check on user-mode return which detects unbalanced …

#### <img src="https://avatars.githubusercontent.com/u/11041719?u=7d465af69debfe56b372670481c551c76b64b293&v=4" width="50">[idl3r](https://github.com/idl3r) opened issue at [2018-06-14 07:57](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/1):

…set_fs issues

#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) commented at [2018-07-04 12:29](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/1#issuecomment-402464800):

Hey, @idl3r, thank you very much!
I'll name it as 'TIF_FSCHECK flag' for easier search, OK?

#### <img src="https://avatars.githubusercontent.com/u/11041719?u=7d465af69debfe56b372670481c551c76b64b293&v=4" width="50">[idl3r](https://github.com/idl3r) commented at [2018-07-04 12:43](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/1#issuecomment-402467942):

Of coz, thank you!

#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) commented at [2018-07-04 15:29](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/1#issuecomment-402510056):

Applied, thanks @idl3r


-------------------------------------------------------------------------------

