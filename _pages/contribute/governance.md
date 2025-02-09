---
title: Governance
section: Contribute
nav-links: true
---

{% include notice icon="info" content='This page describes the *social* structure of [SciJava](/libs/scijava) projects.

-   For information on the *technical* structure, see [Architecture](/develop/architecture).
-   For information on the *legal* structure, see [Licensing](/licensing).' %}


The [ImageJ](/software/imagej) project, and related projects in the [SciJava](/libs/scijava) software ecosystem, are governed as [open source](/licensing/open-source) software projects. Everybody is welcome to [contribute](/contribute) with [plugins](/plugins), patches, [bug reports](/discuss/bugs), [tutorials](/tutorials), [documentation](/learn), and artwork.

That said, every project needs leaders: the ones who participate in *governance* of the project, {% include wikipedia title='Software maintenance' text='maintaining'%} the software and making key decisions.

## Project roles

Because [open source](/licensing/open-source) software (OSS) is highly collaborative, it is extremely important to understand the difference between various roles on the project, to avoid misconceptions about **authority** (who makes decisions) and **responsibility** (who is pledged to do the work) concerning each project.

The most common roles in OSS are:

-   **Founders** are the people who originally launched the project.
-   **Leads** are responsible for making final decisions. In the [open source](/licensing/open-source) world these people are often referred to as [benevolent dictators](http://catb.org/~esr/writings/homesteading/homesteading/ar01s16.html). Changes with a serious impact on the community are typically [discussed on open channels](/discuss) first.
-   **Maintainers** keep the project functional, fix bugs and make releases. They often make day to day decisions, and are typically involved in discussion with the project lead(s) regarding major decisions, although the lead has final decision-making authority.
-   **Developers** are people who work on the project significantly or often. Typically they have direct push access to the source code. In some cases they make day to day decisions, depending on their experience and comfort level with the project.
-   **Contributors** are people who help with the project either currently or in the past. They may participate occasionally or sporadically, and are typically not involved in project decision making.

### SciJava team roles

Projects in the [SciJava component collection](/develop/architecture) define each component's **team** as the group of people who take *responsibility* for it. The following roles formalize the ways people are pledged to help:

| Role        | Commitment                                                                                                                         |
|-------------|------------------------------------------------------------------------------------------------------------------------------------|
| Founder     | Created the project. Does not imply any current participation or responsibility.                                                   |
| Lead        | Has decision-making authority: timing of [releases](/develop/releasing), inclusion of features, etc.                              |
| Developer   | Adds new features or enhancements. Can be assigned to address feature requests.                                                    |
| Debugger    | Fixes [bugs](/develop/project-management#issue-tracking). Can be assigned open [issues](/develop/project-management#issue-tracking) to solve.                                        |
| Reviewer    | Reviews [patch submissions](/contribute).                                                                              |
| Support     | Responds to [community questions](/discuss) and [issue reports](/develop/project-management#issue-tracking). Keeps the issue tracker organized. |
| Maintainer  | Merges [patch submissions](/contribute). Cuts releases.                                                                |
| Contributor | Contributed code to the project. Does not imply any current participation or responsibility.                                       |

Individuals often fill more than one role.

## Component status

This web site documents lots of software [components](/develop/architecture#definitions)—and in particular, many ImageJ [plugins](/plugins). Components in the ecosystem each have a distinct development path, with varying levels of maturity and activity, which is ultimately determined by the people who participate in developing it.

Each component's page features an informational sidebar with a status report derived from the component's declared *team*. This sidebar is intended to help users understand what level to expect when seeking help, reporting issues, and submitting feature requests.

### Development status

**Development status** conveys what to expect regarding a component's future.

| Status   | Meaning                                                                                                                                                                                     |
|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Unstable | Project is under heavy development, with unstable API undergoing iterations of refinement. Typically, these components are either unreleased, or [versioned at 0.x](/develop/versioning). |
| Active   | New features are being actively developed. API breakages are kept as limited as possible.                                                                                                   |
| Stable   | No new features are under development. API is stable.                                                                                                                                       |
| Obsolete | The project is discontinued.                                                                                                                                                                |

### Support status

**Support status** indicates the level to which the team responds to questions and [issue reports](/discuss/bugs).

| Status  | Meaning                                                                                                                                                                                         |
|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Active  | Someone will respond to questions on community channels, and addresses issue reports in the project's issue tracker. A best effort is made to fix reported bugs within a reasonable time frame. |
| Partial | Someone will respond to questions on community channels, as well as to issue reports in the project's issue tracker. But reported bugs may not be addressed in a timely manner.                 |
| Minimal | There is at least one person pledged to the project in some capacity, but not all roles are filled. Response time to questions and issue reports may be protracted.                             |
| None    | No one is pledged to support the project. Questions and issue reports may be ignored.                                                                                                           |

## SciJava project summary

Here is a summary of roles for projects in the [SciJava](/libs/scijava) ecosystem.

{::nomarkdown}
<table>
  <tbody>
    <tr>
      <td>
        <p><strong>Logo</strong></p>
      </td>
      <td>
        <p><strong>Project</strong></p>
      </td>
      <td>
        <p><strong>Founders</strong></p>
      </td>
      <td>
        <p><strong>Leads</strong></p>
      </td>
      <td>
        <p><strong>Maintainers</strong></p>
      </td>
      <td>
        <p><strong>Developers</strong></p>
      </td>
      <td>
        <p><strong>Contributors</strong></p>
      </td>
    </tr>
    <tr>
      <td>
        <p>{% include icon name='SciJava' %}</p>
      </td>
      <td>
        <p><strong><a href="/libs/scijava">SciJava</a></strong></p>
      </td>
      <td>
        <p>{% include person id='joshmoore' %}<br>
        {% include person id='ctrueden' %}</p>
      </td>
      <td>
        <p>{% include person id='ctrueden' %}</p>
      </td>
      <td>
        <p>{% include person id='ctrueden' %}</p>
      </td>
      <td>
        <p><a href="https://github.com/orgs/scijava/people">List on GitHub</a></p>
      </td>
      <td>
        <p>See <a href="/people">Contributors</a></p>
      </td>
    </tr>
    <tr>
      <td>
        <p>{% include icon name='ImgLib2' %}</p>
      </td>
      <td>
        <p><strong><a href="/libs/imglib2">ImgLib2</a></strong></p>
      </td>
      <td>
        <p>{% include person id='axtimwalde' %}<br>
        {% include person id='StephanPreibisch' %}</p>
      </td>
      <td>
        <p>{% include person id='tpietzsch' %}<sup>1</sup><br>
        {% include person id='StephanPreibisch' %}<br>
        {% include person id='axtimwalde' %}</p>
      </td>
      <td>
        <p>{% include person id='tpietzsch' %}<br>
        {% include person id='ctrueden' %}<br>
        {% include person id='StephanPreibisch' %}<br>
        {% include person id='axtimwalde' %}</p>
      </td>
      <td>
        <p><a href="https://github.com/orgs/imglib/people">List on GitHub</a></p>
      </td>
      <td>
        <p>See <a href="/people">Contributors</a></p>
      </td>
    </tr>
    <tr>
      <td>
        <p>{% include icon name='ImageJ1' %}</p>
      </td>
      <td>
        <p><strong><a href="/software/imagej1">ImageJ1</a></strong></p>
      </td>
      <td>
        <p>{% include person id='rasband' %}</p>
      </td>
      <td>
        <p>{% include person id='rasband' %}</p>
      </td>
      <td>
        <p>{% include person id='rasband' %}<br>
        {% include person id='ctrueden' %}</p>
      </td>
      <td>
        <p>{% include person id='rasband' %}</p>
      </td>
      <td>
        <p>See <a href="https://imagej.nih.gov/ij/notes.html">release notes</a></p>
      </td>
    </tr>
    <tr>
      <td>
        <p>{% include icon name='ImageJ2' %}</p>
      </td>
      <td>
        <p><strong><a href="/software/imagej2">ImageJ2</a></strong></p>
      </td>
      <td>
        <p>{% include person id='ctrueden' %}<br>
        {% include person id='eliceiri' %}</p>
      </td>
      <td>
        <p>{% include person id='ctrueden' %}</p>
      </td>
      <td>
        <p>{% include person id='ctrueden' %}</p>
      </td>
      <td>
        <p><a href="https://github.com/orgs/imagej/people">List on GitHub</a></p>
      </td>
      <td>
        <p>See <a href="/people">Contributors</a></p>
      </td>
    </tr>
    <tr>
      <td>
        <p>{% include icon name='SCIFIO' %}</p>
      </td>
      <td>
        <p><strong><a href="/libs/scifio">SCIFIO</a></strong></p>
      </td>
      <td>
        <p>{% include person id='ctrueden' %}<br>
        {% include person id='eliceiri' %}<br>
        {% include person id='hinerm' %}</p>
      </td>
      <td>
        <p>{% include person id='ctrueden' %}</p>
      </td>
      <td>
        <p>{% include person id='ctrueden' %}</p>
      </td>
      <td>
        <p><a href="https://github.com/orgs/scijava/people">List on GitHub</a></p>
      </td>
      <td>
        <p>See <a href="/people">Contributors</a></p>
      </td>
    </tr>
    <tr>
      <td rowspan="3" style="vertical-align: middle">
        <p>{% include icon name='Fiji' %}</p>
      </td>
      <td>
        <p><strong><a href="/software/fiji">Fiji</a></strong></p>
      </td>
      <td>
        <p>{% include person id='dscho' %}<br>
        {% include person id='acardona' %}<br>
        {% include person id='tomancak' %}</p>
      </td>
      <td>
        <p>{% include person id='ctrueden' %}</p>
      </td>
      <td>
        <p>{% include person id='ctrueden' %}<br>
        Gabriella Turek</p>
      </td>
      <td>
        <p><a href="https://github.com/orgs/fiji/people">List on GitHub</a></p>
      </td>
      <td>
        <p>See <a href="/people">Contributors</a></p>
      </td>
    </tr>
    <tr>
      <td>
        <p><strong><a href="/plugins/bdv">BigDataViewer</a></strong></p>
      </td>
      <td>
        <p>{% include person id='tpietzsch' %}</p>
      </td>
      <td>
        <p>{% include person id='tpietzsch' %}</p>
      </td>
      <td>
        <p>{% include person id='tpietzsch' %}<br>
        {% include person id='StephanPreibisch' %}</p>
      </td>
      <td>
        <p><a href="https://github.com/orgs/bigdataviewer/people">List on GitHub</a></p>
      </td>
      <td>
        <p><a href="https://github.com/bigdataviewer/bigdataviewer-core/graphs/contributors">Info on GitHub</a></p>
      </td>
      <td></td>
    </tr>
    <tr>
      <td>
        <p><strong><a href="/plugins/trakem2">TrakEM2</a></strong></p>
      </td>
      <td>
        <p>{% include person id='acardona' %}</p>
      </td>
      <td>
        <p>{% include person id='acardona' %}</p>
      </td>
      <td>
        <p>{% include person id='acardona' %}<br>
        {% include person id='axtimwalde' %}<br>
        {% include person id='ctrueden' %}</p>
      </td>
      <td>
        <p><a href="https://github.com/orgs/trakem2/people">List on GitHub</a></p>
      </td>
      <td>
        <p><a href="https://github.com/trakem2/plugins/trakem2/graphs/contributors">Info on GitHub</a></p>
      </td>
      <td></td>
    </tr>
    <tr>
      <td>
        <p>{% include icon name='SLIM Curve' %}</p>
      </td>
      <td>
        <p><strong><a href="/plugins/slim-curve">SLIM Curve</a></strong></p>
      </td>
      <td>
        <p>Paul Barber<br>
        {% include person id='ctrueden' %}</p>
      </td>
      <td>
        <p>Paul Barber<sup>2</sup><br>
        {% include person id='ctrueden' %}<sup>2</sup></p>
      </td>
      <td>
        <p>{% include person id='aksagar' %}<br>
        {% include person id='ctrueden' %}</p>
      </td>
      <td>
        <p><a href="https://github.com/orgs/slim-curve/people">List on GitHub</a></p>
      </td>
      <td>
        <p><a href="https://github.com/slim-curve/slim-plugin/graphs/contributors">Info on GitHub</a></p>
      </td>
    </tr>
    <tr>
      <td>
        <p>{% include icon name='Bio-Formats' %}</p>
      </td>
      <td>
        <p><strong><a href="/formats/bio-formats">Bio-Formats</a></strong></p>
      </td>
      <td>
        <p>{% include person id='ctrueden' %}<br>
        {% include person id='eliceiri' %}</p>
      </td>
      <td>
        <p>{% include person id='melissalinkert' %}</p>
      </td>
      <td>
        <p>{% include person id='melissalinkert' %}<br>
        {% include person id='sbesson' %}</p>
      </td>
      <td style="white-space: normal">
        <p><a href="https://github.com/openmicroscopy/bioformats/graphs/contributors">List on GitHub</a></p>
      </td>
      <td>
        <p>See <a href="http://www.openmicroscopy.org/site/about/ome-contributors">OME Contributors</a></p>
      </td>
    </tr>
    <tr>
      <td>
        <p>{% include icon name='KNIME' %}</p>
      </td>
      <td>
        <p><strong><a href="/software/knime">KNIME Image Processing</a></strong></p>
      </td>
      <td>
        <p>{% include person id='dietzc' %}<br>
        Martin Horn</p>
      </td>
      <td>
        <p>{% include person id='dietzc' %}</p>
      </td>
      <td>
        <p>{% include person id='dietzc' %}<br>
        Martin Horn</p>
      </td>
      <td>
        <p><a href="https://github.com/orgs/knime-ip/people">List on GitHub</a></p>
      </td>
      <td>
        <p>See <a href="/people">Contributors</a></p>
      </td>
    </tr>
    <tr>
      <td>
        <p>{% include icon name='CellProfiler' %}</p>
      </td>
      <td>
        <p><strong><a href="/software/cellprofiler">CellProfiler</a></strong></p>
      </td>
      <td>
        <p>{% include person id='LeeKamentsky' %}<br>
        Anne Carpenter</p>
      </td>
      <td>
        <p>{% include person id='0x00b1' %}</p>
      </td>
      <td>
        <p>{% include person id='0x00b1' %}</p>
      </td>
      <td>
        <p><a href="https://github.com/orgs/CellProfiler/people">List on GitHub</a></p>
      </td>
      <td>
        <p>See <a href="/people">Contributors</a></p>
      </td>
    </tr>
    <tr>
      <td>
        <p>{% include icon name='OMERO' %}</p>
      </td>
      <td>
        <p><strong><a href="/software/omero">OMERO</a></strong></p>
      </td>
      <td>
        <p>{% include person id='joshmoore' %}<br>
        Jean-Marie Burel<br>
        Chris Allan<br>
        Jason Swedlow</p>
      </td>
      <td>
        <p>{% include person id='joshmoore' %}<br>
        Jean-Marie Burel<br>
        Chris Allan</p>
      </td>
      <td>
        <p><a href="https://github.com/orgs/openmicroscopy/people">List on GitHub</a></p>
      </td>
      <td>
        <p><a href="https://github.com/orgs/openmicroscopy/people">List on GitHub</a></p>
      </td>
      <td>
        <p><a href="https://github.com/openmicroscopy/openmicroscopy/graphs/contributors">Info on GitHub</a></p>
      </td>
    </tr>
    <tr>
      <td>
        <p>{% include icon name='Icy' %}</p>
      </td>
      <td>
        <p><strong><a href="/software/icy">Icy</a></strong></p>
      </td>
      <td>
        <p>Stephane Dallongeville<br>
        {% include person id='Fab14' %}<br>
        Jean-Christophe Olivo-Marin</p>
      </td>
      <td>
        <p>Stephane Dallongeville<br>
        {% include person id='Fab14' %}</p>
      </td>
      <td>
        <p>Stephane Dallongeville<br>
        {% include person id='Fab14' %}</p>
      </td>
      <td>
        <p><a href="https://github.com/orgs/Icy-imaging/people">List on GitHub</a></p>
      </td>
      <td>
        <p><a href="https://github.com/Icy-imaging/Icy-Kernel/graphs/contributors">Info on GitHub</a></p>
      </td>
    </tr>
    <tr>
      <td>
        <p>{% include icon name='Alida' %}</p>
      </td>
      <td>
        <p><strong><a href="/software/alida">Alida</a></strong></p>
      </td>
      <td>
        <p>Stefan Posch<br>
        Birgit Möller</p>
      </td>
      <td>
        <p>Stefan Posch<br>
        Birgit Möller</p>
      </td>
      <td>
        <p>Stefan Posch<br>
        Birgit Möller</p>
      </td>
      <td>
        <p><a href="https://github.com/orgs/alida-hub/people">List on GitHub</a></p>
      </td>
      <td>
        <p><a href="https://github.com/alida-hub/alida/graphs/contributors">Info on GitHub</a></p>
      </td>
    </tr>
    <tr>
      <td>
        <p>{% include icon name='MiToBo' %}</p>
      </td>
      <td>
        <p><strong><a href="/plugins/mitobo">MiToBo</a></strong></p>
      </td>
      <td>
        <p>Stefan Posch<br>
        Birgit Möller</p>
      </td>
      <td>
        <p>Stefan Posch<br>
        Birgit Möller</p>
      </td>
      <td>
        <p>Stefan Posch<br>
        Birgit Möller</p>
      </td>
      <td>
        <p><a href="https://github.com/orgs/mitobo-hub/people">List on GitHub</a></p>
      </td>
      <td>
        <p><a href="https://github.com/mitobo-hub/mitobo/graphs/contributors">Info on GitHub</a></p>
      </td>
    </tr>
  </tbody>
</table>
{:/}

<sup>1</sup> Pietzsch leads on day to day issues. Pietzsch, Preibisch and Saalfeld vote on primary decisions, with Pietzsch's vote breaking ties.  
<sup>2</sup> Barber leads development of the {% include github org='slim-curve' repo='slim-curve' label='SLIM Curve C library' %}; Rueden leads development of the {% include github org='slim-curve' repo='slim-plugin' label='SLIM Curve plugin for ImageJ' %}.

## Further reading

-   [OSS Watch's article on Governance Models](http://oss-watch.ac.uk/resources/governancemodels)
-   Eric S. Raymond's [Homesteading the Noosphere](http://catb.org/~esr/writings/homesteading/homesteading/)
-   Eric S. Raymond's [The Cathedral and the Bazaar](http://www.catb.org/esr/writings/cathedral-bazaar/cathedral-bazaar/)
