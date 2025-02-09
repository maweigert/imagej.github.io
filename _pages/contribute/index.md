---
title: Contributing
section: Contribute
nav-links: true
---

{% include notice icon="osi-symbol" content="The [ImageJ](/software/imagej)
  project, and related projects in the [SciJava](/libs/scijava)
  software ecosystem, are [open source](/licensing/open-source) software
  projects. See the [Licensing](/licensing) page for details." %}

Everybody is welcome to [contribute](/contribute) with [plugins](/plugins),
[patches](/develop/github), [bug reports](/discuss/bugs),
[tutorials](/tutorials), [documentation](/learn), and artwork.

The community encourages discussion about proposed changes on ImageJ's [communication](/discuss/#ways-to-get-help) channels. Submit your ideas!

Start on the [forum](/discuss), searching for discussions related to your contribution to get some context & background. It can also be helpful to [search](/discuss#searching-imagej-resources) for applicable issues and plans across the spectrum of ImageJ resources. The ImageJ community believes that [public discussion is important](/develop/philosophy#open-source) so that ideas are exposed to healthy alternate points of view rather than lost.

If you are interested in helping to tackle open issues, see the [wish list](/develop/wish-list) page.

## This web site

This site, imagej.net, is a wiki (similar to the {% include wikipedia title='Wikipedia' text='Wikipedia'%} project). This site is built by the ImageJ community, and anyone can contribute!

If you are an ImageJ user, contributing documentation is an easy way to give back to the community without needing to learn software development skills.

If you are a plugin author, please consider [documenting your plugin on this site](/contribute/distributing#documenting-your-extension). You can also create your own [tutorial](/tutorials)

When in doubt, discuss your ideas publicly on the Image.sc [Forum](/discuss)!

## ImageJ2

Submit patches to [ImageJ2](/software/imagej2) via [pull requests](https://help.github.com/articles/using-pull-requests/) against [ImageJ2's source on GitHub](https://github.com/imagej).

Note that since ImageJ2 has a modular [architecture](/develop/architecture), it is possible that your change would be more applicable to one of the supporting technologies such as [ImgLib2](https://github.com/imglib), [SCIFIO](https://github.com/scifio) or [SciJava](https://github.com/scijava).

## ImageJ1

Changes to [ImageJ1](/software/imagej1) are made by {% include person id='rasband' %}, who is the sole [ImageJ1](/software/imagej1) developer. He takes patch submissions and then reworks them to fit within the project's development model and style before merging them. Attribution for the changes is noted in the release notes (see [ImageJ1's Release Notes/News](https://imagej.nih.gov/ij/notes.html)).

Methods of getting the patch to Wayne include:

-   Send the modified code in a [private mail to Wayne](mailto:rasbandw@mail.nih.gov). He prefers not to discuss code on the ImageJ mailing list.
-   Send a patch to [Wayne via private mail](mailto:rasbandw@mail.nih.gov). It should apply to [the latest revision](https://github.com/imagej/imagej1/commit/master) and must not use any Git extension because Wayne uses `patch(1)` to apply the patch.
-   Submit a pull request on GitHub against {% include github org='imagej' repo='imagej1' label='the ImageJ1 repository' %}. Please note, though, that none of [the past Pull Requests](https://github.com/imagej/imagej1/pulls?q=is%3Apr+is%3Aclosed) were merged using the standard Git workflow.

The important part is that Wayne receive the code/patch, since he is the only one with the authority to merge it.

(See also this [nice response](http://imagej.1557.x6.nabble.com/Non-Uniform-X-Y-Z-Units-Patch-tp5008492p5008496.html) from Curtis Rueden on the ImageJ mailing list.)
