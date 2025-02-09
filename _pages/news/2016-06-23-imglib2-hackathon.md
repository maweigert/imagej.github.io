---
title: 2016-06-23 - ImgLib2 hackathon
---

{% capture photocaption %}
[ImgLib2](/libs/imglib2) hackers, from left to right: {% include person id='axtimwalde' %}, {% include person id='dietzc' %}, {% include person id='ctrueden' %}, {% include person id='tpietzsch' %}.
{% endcapture %}

{% include img align="right" src="/media/news/janelia-2016-hackathon.jpg" caption=photocaption width="450px" %} From Sunday, June 19, 2016 through Tuesday, June 28, 2016, {% include person id='axtimwalde' %} at HHMI Janelia in Ashburn, Virginia hosted {% include person id='tpietzsch' %}, {% include person id='ctrueden' %} and {% include person id='dietzc' %} for a [hackathon](/events/hackathons) to improve the core [ImageJ2](/software/imagej2) data model.

## Timeline

The following chart illustrates when everyone was present:

{% include img name="timeline" src="/media/news/imglib2-hackathon-timeline.png" %}

<!-- The image above was rendered by MediaWiki. Original data, as converted, follows.

<timeline> Preset = TimeHorizontal\_AutoPlaceBars\_UnitYear

ImageSize = width:550

Colors =

` id:c01 value:blue`  
` id:c02 value:red`  
` id:c03 value:green`  
` id:c04 value:black`  
` id:c05 value:magenta`  
` id:gridLine value:gray(0.5)`  
` id:gridCanvas value:gray(0.8)`

BackgroundColors = canvas:gridCanvas

Period = from:19 till:28

ScaleMajor = unit:year increment:1 start:19 grid:white

LineData =

` at:19 color:gridLine layer:back width:0.5`  
` at:20 color:gridLine layer:back width:0.5`  
` at:21 color:gridLine layer:back width:0.5`  
` at:22 color:gridLine layer:back width:0.5`  
` at:23 color:gridLine layer:back width:0.5`  
` at:24 color:gridLine layer:back width:0.5`  
` at:25 color:gridLine layer:back width:0.5`  
` at:26 color:gridLine layer:back width:0.5`  
` at:27 color:gridLine layer:back width:0.5`  
` at:28 color:gridLine layer:back width:0.5`

BarData=

` barset:Hackers`

PlotData=

` width:15`  
` fontsize:M`  
` textcolor:white`  
` align:left `  
` anchor:from `  
` shift:(4,-4) `  
` color:black`  
` barSet:Hackers`

` color:c01 from:19 till:28 text:"Stephan Saalfeld (Janelia)"`  
` color:c02 from:20 till:28 text:"Tobias Pietzsch (MPI-CBG)"`  
` color:c03 from:20 till:27 text:"Christian Dietz (UniKN)"`  
` color:c04 from:19 till:24 text:"Curtis Rueden (UniWisc)"`

</timeline>

-->

And {% include person id='kephale' %} stopped by on the afternoon of the 23rd to discuss some issues for a bit.

## Discussions and progress

We spent the first couple of days discussing use cases and requirements for core [ImageJ2](/software/imagej2) metadata-rich image structures.

Some of the needed features we considered:

-   [ImageJ1](/software/imagej1) style calibration
-   Time space shearing
-   Transformations for display
-   Color conversion for display
-   Intensity conversion for display
-   Store preprocessed information about raw data (e.g.: min, max, histogram, features, ...)
-   Projections
-   Non-linear axes transformations (e.g.: log)
-   Other arbitrary transformations (e.g.: polar to cartesian)
-   Some informative strings (metadata)

The central use cases we used as litmus tests for our ideas:

-   Interpolate differently across different dimensions of an image (e.g.: treat XYZ as continuous with {% include javadoc project='ImgLib2' package='net/imglib2/interpolation/randomaccess' class='LanczosInterpolator' label='Lanczos' %}, but time as discrete with {% include javadoc project='ImgLib2' package='net/imglib2/interpolation/randomaccess' class='NearestNeighborInterpolator' label='nearest neighbor' %} or maybe {% include javadoc project='ImgLib2' package='net/imglib2/interpolation/randomaccess' class='NLinearInterpolator' label='1D linear' %}.
-   Register 2D tiles into a larger 2D image, a la [TrakEM2](/plugins/trakem2).
-   Register 2D planes over time (e.g.: correct for shaky cam).
-   Time skew over samples: each plane/scanline/pixel can have a physical timestamp, which could be taken into account somehow, although in practice doing so is very rare.

We spent quite some time considering how to create a unified data structure (`SpaceTree`? :-) which would keep multiple images grouped into some tree- or graph-like structure, with attached {% include javadoc project='ImgLib2' package='net/imglib2/realtransform' class='RealTransform' label='transforms' %}, in a way which handled all of the above use cases and more.

Ultimately, we concluded it was too difficult to design such a data structure in a vacuum, so we instead opted to start coding individual use cases using [ImgLib2](/libs/imglib2). We came up with a design which provides: A) a container for metadata-rich images, with individual metadata elements sensitive to {% include javadoc project='ImgLib2' package='net/imglib2/view' class='Views' %} manipulations; and B) a scheme for "grooming" these metadata-rich images into increasingly useful data structures depending on the use case.

### Metadata-rich containers

We invented `MetaViews`, a class analogous to {% include javadoc project='ImgLib2' package='net/imglib2/view' class='Views' %} but for metadata elements. These elements are then aggregated into a `MetaSpace`, which provides mutators and accessors for working with the space. For example, you might attach a physical calibration to a spatial axis, or a custom variable-width timestamp to each point along the time axis.

On top of this, we are actively developing a (tentatively named) `RichImage` structure, consisting of image pixels (e.g., a `RandomAccessible`) together with a `MetaSpace` which describes it. The [SCIFIO](/libs/scifio) library will be updated to produce a `Dataset`, a collection of `RichImage` elements, which are populated with elements directly from the source data format.

### Image enrichers

For the various use cases above (e.g., registration of tiles in a larger space), we will have a pipeline which calls `ImageInspector` plugins that examine the `RichImages` and decide whether they can be improved with available `ImageEnricher` plugins. (Again: these are working names only.) For instance, the `Dataset` image collection might consist of a bag of 2D tiles with associated affine transforms; a `MosaicEnricher` could then apply those transforms virtually to offer the larger 2D mosaic image as a `RandomAccessible` which returns lists of tile values at each sample. A `FusionEnricher` could operate on any `RandomAccessible` which returns a list, blending the values of that list using a given `Converter`. (In practice, the enrichers will very likely be more general than these examples, but hopefully they serve here to illustrate some of the possibilities.)

### Source code

Current work on these data structures can be seen at:

- [https://github.com/imagej/imagej-common/compare/rich](https://github.com/imagej/imagej-common/compare/rich)

There is also an experimental repository for the hackathon at:

- [https://github.com/imagej/janelia-hackathon-2016](https://github.com/imagej/janelia-hackathon-2016)

  
