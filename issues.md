Export of Github issues for [a13xp0p0v/linux-kernel-defence-map](https://github.com/a13xp0p0v/linux-kernel-defence-map).

# [\#11 Issue](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/11) `open`: RET2DIR Mitigated by CFI -- Kernel Discussions on CONFIG_XPFO
**Labels**: `bug`


#### <img src="https://avatars.githubusercontent.com/u/158655396?v=4" width="50">[wryMitts](https://github.com/wryMitts) opened issue at [2024-02-22 00:31](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/11):

After reading these conversations, per Torvalds and other devs it seems that XPFO's defense on ret2dir is less of a code execution threat, and more a data exposure threat. The term was coined "read2dir" instead. 

Thoughts on the map, perhaps move ret2dir under CFI protections, but also split to have read2dir? 

> On Thu, Apr 18, 2019 at 7:35 AM Khalid Aziz <khalid.aziz@oracle.com> wrote:
> >
> > On 4/17/19 11:41 PM, Kees Cook wrote:
> > > On Wed, Apr 17, 2019 at 11:41 PM Andy Lutomirski <luto@kernel.org> wrote:
> > >> I don't think this type of NX goof was ever the argument for XPFO.
> > >> The main argument I've heard is that a malicious user program writes a
> > >> ROP payload into user memory (regular anonymous user memory) and then
> > >> gets the kernel to erroneously set RSP (*not* RIP) to point there.
> > >
> > > Well, more than just ROP. Any of the various attack primitives. The NX
> > > stuff is about moving RIP: SMEP-bypassing. But there is still basic
> > > SMAP-bypassing for putting a malicious structure in userspace and
> > > having the kernel access it via the linear mapping, etc.
> > >
> > >> I find this argument fairly weak for a couple reasons.  First, if
> > >> we're worried about this, let's do in-kernel CFI, not XPFO, to
> > >
> > > CFI is getting much closer. Getting the kernel happy under Clang, LTO,
> > > and CFI is under active development. (It's functional for arm64
> > > already, and pieces have been getting upstreamed.)
> > >
> >
> > CFI theoretically offers protection with fairly low overhead. I have not
> > played much with CFI in clang. I agree with Linus that probability of
> > bugs in XPFO implementation itself is a cause of concern. If CFI in
> > Clang can provide us the same level of protection as XPFO does, I
> > wouldn't want to push for an expensive change like XPFO.
> >
> > If Clang/CFI can't get us there for extended period of time, does it
> > make sense to continue to poke at XPFO?
> 
> Well, I think CFI will certainly vastly narrow the execution paths
> available to an attacker, but what I continue to see XPFO useful for
> is stopping attacks that need to locate something in memory. (i.e. not
> ret2dir but, like, read2dir.) It's arguable that such attacks would
> just use heap, stack, etc to hold such things, but the linear map
> remains relatively easy to find/target. But I agree: the protection is
> getting more and more narrow (especially with CFI coming down the
> pipe), and if it's still a 28% hit, that's not going to be tenable for
> anyone but the truly paranoid. :)
> 
> All that said, there isn't a very good backward-edge CFI protection
> (i.e. ROP defense) on x86 in Clang. The forward-edge looks decent, but
> requires LTO, etc. My point is there is still a long path to gaining
> CFI in upstream.
> 


Related emails with Torvalds and other kernel dev comments:
https://lore.kernel.org/all/CAHk-=whUwOjFW6RjHVM8kNOv1QVLJuHj2Dda0=mpLPdJ1UyatQ@mail.gmail.com/
https://lore.kernel.org/all/CALCETrXm9PuUTEEmzA8bQJmg=PHC_2XSywECittVhXbMJS1rSA@mail.gmail.com/
https://lore.kernel.org/all/CAGXu5jL-qJtW7eH8S2yhqciE+J+FWz8HHzTrGJTgVUbd55n6dQ@mail.gmail.com/
https://lore.kernel.org/all/8f9d059d-e720-cd24-faa6-45493fc012e0@oracle.com/
https://lore.kernel.org/all/302e3d5b-d2fd-3c25-335b-466ba83035c5@oracle.com/
https://lore.kernel.org/all/CAGXu5jLPkD_6BL1m2=13KVTfZ7znr-xAyz+CB23eoeboFgCSOg@mail.gmail.com/#t


#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) commented at [2024-02-25 21:56](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/11#issuecomment-1963074254):

Hi @wryMitts,
Thanks for creating this issue!

As I understand the terminology in this LKML discussion,  they use:
 -  `ret2dir` for executing exploit payload using the direct mapping,
 -  `read2dir` for getting data needed for exploit using the direct mapping. On the Linux Kernel Defence Map, we already have `ret2dir + ROP/JOP/COP`, but `read2dir` would be a more generic term.

Let's see whether `read2dir` become a popular term (for adding it in future).

Please see the commit https://github.com/a13xp0p0v/linux-kernel-defence-map/commit/02cd95c11eb33a76e607c05f8a218c856b12865a, where I fixed the link from `cluster_cfi`. Now CFI is connected to all control flow hijack techniques, which is more correct. Thank you!

#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) commented at [2024-03-11 14:16](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/11#issuecomment-1988548475):

Hmmm...

Maybe we need to add something like `Userspace Data Access via direct mapping`  (or linear mapping?) instead of `read2dir` to show that the `XPFO` approach provides additional security effects comparing to CFI.

But this concept looks very long.


-------------------------------------------------------------------------------

# [\#10 Issue](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/10) `closed`: Please update CFI_CLANG with kCFI Updates for Forward & Backward Edge
**Labels**: `enhancement`


#### <img src="https://avatars.githubusercontent.com/u/158655396?v=4" width="50">[wryMitts](https://github.com/wryMitts) opened issue at [2024-02-17 09:07](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/10):

kCFI was merged (6.1 ?) and is now used by default when selecting CFI CLANG

https://lwn.net/Articles/893164/

It is fine grained forward and backward edge:

Page 2:

> As a step towards practical and effective kernel-level control-flow integrity for commodity operating systems,
> in this paper we present kCFI, a fine-grained CFI scheme for the Linux kernel. The proposed approach combines
> the benefits of a tag-based, fine-grained CFI policy enforcement for both forward and backward edges which offers
> increased protection compared to coarse-grained CFI,

https://www.blackhat.com/docs/asia-17/materials/asia-17-Moreira-Drop-The-Rop-Fine-Grained-Control-Flow-Integrity-For-The-Linux-Kernel-wp.pdf

#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) commented at [2024-02-25 19:51](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/10#issuecomment-1963042861):

@wryMitts, thanks for creating this issue.

Please see the commit  https://github.com/a13xp0p0v/linux-kernel-defence-map/commit/05916b0d9994f0df60ab0b2556047ee1070d1978. 

`ARM64: CFI_CLANG` is renamed to `CFI_CLANG (KCFI)`.

`ARM64` is removed from the name because this feature is supported both for `X86_64` and `ARM64`.

As the documentation says, "KCFI is a proposed forward-edge control-flow integrity scheme for Clang", so I didn't add the link to the "Backward-edge CFI" node.

#### <img src="https://avatars.githubusercontent.com/u/158655396?v=4" width="50">[wryMitts](https://github.com/wryMitts) commented at [2024-02-26 05:04](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/10#issuecomment-1963325141):

After extended review, looks like there is two implementations with the name kCFI, one merged, other never made it into the kernel. Made by two different people. That seems to be why documentation was confusing. My mistake


-------------------------------------------------------------------------------

# [\#9 PR](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/9) `merged`: Zoom out: Drop the redundant details of the mitigations of the CPU bugs

#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) opened issue at [2023-12-09 22:06](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/9):

Refers to #8 

#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) commented at [2023-12-09 22:10](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/9#issuecomment-1848734391):

Now need to add the cluster with options connected to the "mitigations" cmdline parameter.


-------------------------------------------------------------------------------

# [\#8 Issue](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/8) `closed`: Drop the redundant details of the mitigations of the CPU bugs
**Labels**: `enhancement`


#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) opened issue at [2023-12-09 20:17](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/8):

Zoom out a bit. Some details about the mitigations of CPU bugs are not needed.

#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) commented at [2024-02-05 00:27](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/8#issuecomment-1926012775):

This work is done:
https://github.com/a13xp0p0v/linux-kernel-defence-map/commit/d010f4b5258048076f2e1cb10ff8b34ef7e7ea1b


-------------------------------------------------------------------------------

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

#### <img src="https://avatars.githubusercontent.com/u/1419667?u=de82e29061c3ef5f1c19f95528f8a82b08051fd2&v=4" width="50">[a13xp0p0v](https://github.com/a13xp0p0v) commented at [2024-03-15 23:00](https://github.com/a13xp0p0v/linux-kernel-defence-map/issues/6#issuecomment-2000606823):

One more interesting example of graph visualization: https://www.highcharts.com/demo/highcharts/network-graph


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

