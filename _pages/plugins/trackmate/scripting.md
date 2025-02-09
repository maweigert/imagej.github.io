---
mediawiki: Scripting_TrackMate
title: Scripting TrackMate
---

## TrackMate scripting principle

[TrackMate\_](/plugins/trackmate) can also be used without the GUI, using a scripting language that allows making calls to Java. The most simple way to get started is to use the [Script Editor](/scripting/script-editor), which takes care of the difficult & boring part for you (such as path). The examples proposed on this page all use Jython, but can be adapted to anything.

Since we are calling the internals of TrackMate, we must get to know a bit of its guts. I have tried to come up with a rational design; though not always successfully. There is 3 main classes to interact with in a script:

-   Model ([`fiji.plugin.trackmate.Model`](https://fiji.sc/javadoc/fiji/plugin/trackmate/Model.html)) is the class in charge of <u>storing the data</u>. It cannot do anything to create it. It can help you follow manual modifications you would made in the manual editing mode, interrogate it, ... but it is conceptually just a data recipient.

<!-- -->

-   Settings ([`fiji.plugin.trackmate.Settings`](https://fiji.sc/javadoc/fiji/plugin/trackmate/Settings.html)) is the class storing the fields that will configure TrackMate and pilot how the data is created. This is where you specify what is the source image, what are the detector and tracking algorithms to use, what are the filters to use, etc...

<!-- -->

-   TrackMate (\[https://fiji.sc/javadoc/fiji/plugin/trackmate/plugins/trackmate.html>`fiji.plugin.trackmate.TrackMate`\]) is the guy that does the actual work. In scripts, we use it to actually <u>perform the analysis tasks</u, such as generating spots from images, linking them into track, etc... It reads configuration information in the Settings object mentioned above and put the resulting data in the model.

So getting a working script is all about configuring a proper `Settings` object and calling `exec*` methods on a `TrackMate` object. Then we read the results in the `Model` object.

## A full example

Here is an example of full tracking process, using the easy image found in the [first tutorial](/plugins/trackmate/getting-started). The following (Jython) script works as following:

-   It fetches the image from the web
-   It configures settings for segmentation and tracking
-   The model is instantiated, with the settings and imp objects
-   The [TrackMate](/plugins/trackmate) class is instantiated with the model object
-   Then the [TrackMate](/plugins/trackmate) object performs all the steps needed.
-   The final results is displayed as an overlay.

<!-- -->


    import sys

    from ij import IJ
    from ij import WindowManager

    from fiji.plugin.trackmate import Model
    from fiji.plugin.trackmate import Settings
    from fiji.plugin.trackmate import TrackMate
    from fiji.plugin.trackmate import SelectionModel
    from fiji.plugin.trackmate import Logger
    from fiji.plugin.trackmate.detection import LogDetectorFactory
    from fiji.plugin.trackmate.tracking import LAPUtils
    from fiji.plugin.trackmate.tracking.sparselap import SparseLAPTrackerFactory
    from fiji.plugin.trackmate.providers import SpotAnalyzerProvider
    from fiji.plugin.trackmate.providers import EdgeAnalyzerProvider
    from fiji.plugin.trackmate.providers import TrackAnalyzerProvider
    import fiji.plugin.trackmate.visualization.hyperstack.HyperStackDisplayer as HyperStackDisplayer
    import fiji.plugin.trackmate.features.FeatureFilter as FeatureFilter

    # Get currently selected image
    imp = WindowManager.getCurrentImage()
    # imp = IJ.openImage('https://fiji.sc/samples/FakeTracks.tif')
    imp.show()


    #----------------------------
    # Create the model object now
    #----------------------------

    # Some of the parameters we configure below need to have
    # a reference to the model at creation. So we create an
    # empty model now.

    model = Model()

    # Send all messages to ImageJ log window.
    model.setLogger(Logger.IJ_LOGGER)



    #------------------------
    # Prepare settings object
    #------------------------

    settings = Settings()
    settings.setFrom(imp)

    # Configure detector - We use the Strings for the keys
    settings.detectorFactory = LogDetectorFactory()
    settings.detectorSettings = {
        'DO_SUBPIXEL_LOCALIZATION' : True,
        'RADIUS' : 2.5,
        'TARGET_CHANNEL' : 1,
        'THRESHOLD' : 0.,
        'DO_MEDIAN_FILTERING' : False,
    }  

    # Configure spot filters - Classical filter on quality
    filter1 = FeatureFilter('QUALITY', 30, True)
    settings.addSpotFilter(filter1)

    # Configure tracker - We want to allow merges and fusions
    settings.trackerFactory = SparseLAPTrackerFactory()
    settings.trackerSettings = LAPUtils.getDefaultLAPSettingsMap() # almost good enough
    settings.trackerSettings['ALLOW_TRACK_SPLITTING'] = True
    settings.trackerSettings['ALLOW_TRACK_MERGING'] = True

    # Add ALL the feature analyzers known to TrackMate, via
    # providers.
    # They offer automatic analyzer detection, so all the
    # available feature analyzers will be added.

    spotAnalyzerProvider = SpotAnalyzerProvider()
    for key in spotAnalyzerProvider.getKeys():
        print( key )
        settings.addSpotAnalyzerFactory( spotAnalyzerProvider.getFactory( key ) )

    edgeAnalyzerProvider = EdgeAnalyzerProvider()
    for  key in edgeAnalyzerProvider.getKeys():
        print( key )
        settings.addEdgeAnalyzer( edgeAnalyzerProvider.getFactory( key ) )

    trackAnalyzerProvider = TrackAnalyzerProvider()
    for key in trackAnalyzerProvider.getKeys():
        print( key )
        settings.addTrackAnalyzer( trackAnalyzerProvider.getFactory( key ) )

    # Configure track filters - We want to get rid of the two immobile spots at
    # the bottom right of the image. Track displacement must be above 10 pixels.

    filter2 = FeatureFilter('TRACK_DISPLACEMENT', 10, True)
    settings.addTrackFilter(filter2)


    #-------------------
    # Instantiate plugin
    #-------------------

    trackmate = TrackMate(model, settings)

    #--------
    # Process
    #--------

    ok = trackmate.checkInput()
    if not ok:
        sys.exit(str(trackmate.getErrorMessage()))

    ok = trackmate.process()
    if not ok:
        sys.exit(str(trackmate.getErrorMessage()))


    #----------------
    # Display results
    #----------------

    selectionModel = SelectionModel(model)
    displayer =  HyperStackDisplayer(model, selectionModel, imp)
    displayer.render()
    displayer.refresh()

    # Echo results with the logger we set at start:
    model.getLogger().log(str(model))

## Loading and reading from a saved TrackMate XML file

Scripting is a good way to interrogate and play non-interactively with tracking results. The example below shows how to load a XML TrackMate file and rebuild a full working model from it.

That way you could for instance redo a full tracking process by only changing one parameter with respect to the saved one. You might also want to check results without relying on the GUI. Etc...

For the example below to work for you, you will have to edit line 20 and put the actual path to your TrackMate file.

    from fiji.plugin.trackmate.visualization.hyperstack import HyperStackDisplayer
    from fiji.plugin.trackmate.io import TmXmlReader
    from fiji.plugin.trackmate import Logger
    from fiji.plugin.trackmate import Settings
    from fiji.plugin.trackmate import SelectionModel
    from fiji.plugin.trackmate.providers import DetectorProvider
    from fiji.plugin.trackmate.providers import TrackerProvider
    from fiji.plugin.trackmate.providers import SpotAnalyzerProvider
    from fiji.plugin.trackmate.providers import EdgeAnalyzerProvider
    from fiji.plugin.trackmate.providers import TrackAnalyzerProvider
    from java.io import File
    import sys


    #----------------
    # Setup variables
    #----------------

    # Put here the path to the TrackMate file you want to load
    file = File('/Users/tinevez/Desktop/Data/FakeTracks.xml')

    # We have to feed a logger to the reader.
    logger = Logger.IJ_LOGGER

    #-------------------
    # Instantiate reader
    #-------------------

    reader = TmXmlReader(file)
    if not reader.isReadingOk():
        sys.exit(reader.getErrorMessage())
    #-----------------
    # Get a full model
    #-----------------

    # This will return a fully working model, with everything
    # stored in the file. Missing fields (e.g. tracks) will be
    # null or None in python
    model = reader.getModel()
    # model is a fiji.plugin.trackmate.Model

    #----------------
    # Display results
    #----------------

    # We can now plainly display the model. It will be shown on an
    # empty image with default magnification.
    sm = SelectionModel(model)
    displayer =  HyperStackDisplayer(model, sm)
    displayer.render()

    #---------------------------------------------
    # Get only part of the data stored in the file
    #---------------------------------------------

    # You might want to access only separate parts of the
    # model.

    spots = model.getSpots()
    # spots is a fiji.plugin.trackmate.SpotCollection

    logger.log(str(spots))

    # If you want to get the tracks, it is a bit trickier.
    # Internally, the tracks are stored as a huge mathematical
    # simple graph, which is what you retrieve from the file.
    # There are methods to rebuild the actual tracks, taking
    # into account for everything, but frankly, if you want to
    # do that it is simpler to go through the model:

    trackIDs = model.getTrackModel().trackIDs(True) # only filtered out ones
    for id in trackIDs:
        logger.log(str(id) + ' - ' + str(model.getTrackModel().trackEdges(id)))


    #---------------------------------------
    # Building a settings object from a file
    #---------------------------------------

    # Reading the Settings object is actually currently complicated. The
    # reader wants to initialize properly everything you saved in the file,
    # including the spot, edge, track analyzers, the filters, the detector,
    # the tracker, etc...
    # It can do that, but you must provide the reader with providers, that
    # are able to instantiate the correct TrackMate Java classes from
    # the XML data.

    # We start by creating an empty settings object
    settings = Settings()

    # Then we create all the providers, and point them to the target model:
    detectorProvider        = DetectorProvider()
    trackerProvider         = TrackerProvider()
    spotAnalyzerProvider    = SpotAnalyzerProvider()
    edgeAnalyzerProvider    = EdgeAnalyzerProvider()
    trackAnalyzerProvider   = TrackAnalyzerProvider()

    # Ouf! now we can flesh out our settings object:
    reader.readSettings(settings, detectorProvider, trackerProvider, spotAnalyzerProvider, edgeAnalyzerProvider, trackAnalyzerProvider)

    logger.log(str('\n\nSETTINGS:'))
    logger.log(str(settings))

    # The settings object is also instantiated with the target image.
    # Note that the XML file only stores a link to the image.
    # If the link is not valid, the image will not be found.
    imp = settings.imp
    imp.show()

    # With this, we can overlay the model and the source image:
    displayer =  HyperStackDisplayer(model, sm, imp)
    displayer.render()

## Export spot, edge and track numerical features after tracking

This example shows how to extract numerical features from tracking results.

TrackMate computes and stores three kind of numerical features:

-   Spot features, such as a spot location (X, Y, Z), its mean intensity, radius etc...
-   Edge or link features: An edge is a link between two spots. Its feature typically stores the velocity and displacement, which are defined only for two consecutive spots in the same track.
-   Track features: numerical features that apply to a whole track, such as the number of spots it contains.

By default, TrackMate only computes a very limited number of features. The GUI forces TrackMate to compute them all, but if you do scripting, you will have to explicitly configures TrackMate to compute the features you desire. This is done by adding feature analyzers to the settings object.

There are some gotchas: some feature analyzers require other numerical features to be already calculated. If something does not work, it is a good idea to directly check the preamble in the source code of the analyzers ([TrackMate feature logic](https://github.com/fiji/plugins/trackmate/blob/master/src/main/java/fiji/plugin/trackmate/features/)).

Finally, depending on their type, numerical features are not stored at the same place:

-   Spot features are simply conveyed by the spot object, and you can access them through `spot.getFeature('FEATURE_NAME')`
-   Edge and track features are stored in a sub-component of the model object called the FeatureModel ({% include github repo='fiji' branch='master' path='src-plugins/plugins/trackmate\_/src/main/java/fiji/plugin/trackmate/FeatureModel.java' label='FeatureModel.java' %}).

Check the script below to see a working example.

    from ij import IJ, ImagePlus, ImageStack
    import fiji.plugin.trackmate.Settings as Settings
    import fiji.plugin.trackmate.Model as Model
    import fiji.plugin.trackmate.SelectionModel as SelectionModel
    import fiji.plugin.trackmate.TrackMate as TrackMate
    import fiji.plugin.trackmate.Logger as Logger
    import fiji.plugin.trackmate.detection.DetectorKeys as DetectorKeys
    import fiji.plugin.trackmate.detection.DogDetectorFactory as DogDetectorFactory
    import fiji.plugin.trackmate.tracking.sparselap.SparseLAPTrackerFactory as SparseLAPTrackerFactory
    import fiji.plugin.trackmate.tracking.LAPUtils as LAPUtils
    import fiji.plugin.trackmate.visualization.hyperstack.HyperStackDisplayer as HyperStackDisplayer
    import fiji.plugin.trackmate.features.FeatureFilter as FeatureFilter
    import fiji.plugin.trackmate.features.FeatureAnalyzer as FeatureAnalyzer
    import fiji.plugin.trackmate.features.spot.SpotContrastAndSNRAnalyzerFactory as SpotContrastAndSNRAnalyzerFactory
    import fiji.plugin.trackmate.action.ExportStatsToIJAction as ExportStatsToIJAction
    import fiji.plugin.trackmate.io.TmXmlReader as TmXmlReader
    import fiji.plugin.trackmate.action.ExportTracksToXML as ExportTracksToXML
    import fiji.plugin.trackmate.io.TmXmlWriter as TmXmlWriter
    import fiji.plugin.trackmate.features.ModelFeatureUpdater as ModelFeatureUpdater
    import fiji.plugin.trackmate.features.SpotFeatureCalculator as SpotFeatureCalculator
    import fiji.plugin.trackmate.features.spot.SpotContrastAndSNRAnalyzer as SpotContrastAndSNRAnalyzer
    import fiji.plugin.trackmate.features.spot.SpotIntensityAnalyzerFactory as SpotIntensityAnalyzerFactory
    import fiji.plugin.trackmate.features.track.TrackSpeedStatisticsAnalyzer as TrackSpeedStatisticsAnalyzer
    import fiji.plugin.trackmate.util.TMUtils as TMUtils


    # Get currently selected image
    #imp = WindowManager.getCurrentImage()
    imp = IJ.openImage('https://fiji.sc/samples/FakeTracks.tif')
    #imp.show()


    #-------------------------
    # Instantiate model object
    #-------------------------

    model = Model()

    # Set logger
    model.setLogger(Logger.IJ_LOGGER)

    #------------------------
    # Prepare settings object
    #------------------------

    settings = Settings()
    settings.setFrom(imp)

    # Configure detector
    settings.detectorFactory = DogDetectorFactory()
    settings.detectorSettings = {
        DetectorKeys.KEY_DO_SUBPIXEL_LOCALIZATION : True,
        DetectorKeys.KEY_RADIUS : 2.5,
        DetectorKeys.KEY_TARGET_CHANNEL : 1,
        DetectorKeys.KEY_THRESHOLD : 5.,
        DetectorKeys.KEY_DO_MEDIAN_FILTERING : False,
    }

    # Configure tracker
    settings.trackerFactory = SparseLAPTrackerFactory()
    settings.trackerSettings = LAPUtils.getDefaultLAPSettingsMap()
    settings.trackerSettings['LINKING_MAX_DISTANCE'] = 10.0
    settings.trackerSettings['GAP_CLOSING_MAX_DISTANCE']=10.0
    settings.trackerSettings['MAX_FRAME_GAP']= 3

    # Add the analyzers for some spot features.
    # You need to configure TrackMate with analyzers that will generate
    # the data you need.
    # Here we just add two analyzers for spot, one that computes generic
    # pixel intensity statistics (mean, max, etc...) and one that computes
    # an estimate of each spot's SNR.
    # The trick here is that the second one requires the first one to be in
    # place. Be aware of this kind of gotchas, and read the docs.
    settings.addSpotAnalyzerFactory(SpotIntensityAnalyzerFactory())
    settings.addSpotAnalyzerFactory(SpotContrastAndSNRAnalyzerFactory())

    # Add an analyzer for some track features, such as the track mean speed.
    settings.addTrackAnalyzer(TrackSpeedStatisticsAnalyzer())

    settings.initialSpotFilterValue = 1

    print(str(settings))

    #----------------------
    # Instantiate trackmate
    #----------------------

    trackmate = TrackMate(model, settings)

    #------------
    # Execute all
    #------------


    ok = trackmate.checkInput()
    if not ok:
        sys.exit(str(trackmate.getErrorMessage()))

    ok = trackmate.process()
    if not ok:
        sys.exit(str(trackmate.getErrorMessage()))



    #----------------
    # Display results
    #----------------

    model.getLogger().log('Found ' + str(model.getTrackModel().nTracks(True)) + ' tracks.')

    selectionModel = SelectionModel(model)
    displayer =  HyperStackDisplayer(model, selectionModel, imp)
    displayer.render()
    displayer.refresh()

    # The feature model, that stores edge and track features.
    fm = model.getFeatureModel()

    for id in model.getTrackModel().trackIDs(True):

        # Fetch the track feature from the feature model.
        v = fm.getTrackFeature(id, 'TRACK_MEAN_SPEED')
        model.getLogger().log('')
        model.getLogger().log('Track ' + str(id) + ': mean velocity = ' + str(v) + ' ' + model.getSpaceUnits() + '/' + model.getTimeUnits())

        track = model.getTrackModel().trackSpots(id)
        for spot in track:
            sid = spot.ID()
            # Fetch spot features directly from spot.
            x=spot.getFeature('POSITION_X')
            y=spot.getFeature('POSITION_Y')
            t=spot.getFeature('FRAME')
            q=spot.getFeature('QUALITY')
            snr=spot.getFeature('SNR')
            mean=spot.getFeature('MEAN_INTENSITY')
            model.getLogger().log('\tspot ID = ' + str(sid) + ': x='+str(x)+', y='+str(y)+', t='+str(t)+', q='+str(q) + ', snr='+str(snr) + ', mean = ' + str(mean))

## Manually creating a model

TrackMate aims at combining automatic and manual tracking facilities. This is also the case when scripting: a part of the API offers to a edit a model extensively. A few code patterns must be followed.

First, every edit must happen between a call to `model.beginUpdate()` and `model.endUpdate()`:

    model.beginUpdate()
    # ... do whatever you want to the model here.
    model.endUpdate()

The reason for this is that TrackMate caches each modification made to its model. This is required because we can deal with a rather complex content. For instance: imagine you have a single track that splits in two branches at some point. If you decide to remove the spot at the fork, a complex series of events will happen:

-   First, three edges will be removed: the ones that were connected to the spot you just removed.
-   Then the spot will actually be removed from the model.
-   But then you need to recompute the tracks, because now, you have 3 tracks instead of 1.
-   But also: all the numerical features of the tracks are now invalid, and you need to recompute them.
-   And what happens to the track name? What track, amongst the 3 new ones, will receive the old name?

Well, TrackMate does that for you automatically, but for the chain of events to happen timely, you must make your edits within this `model.beginUpdate() / model.endUpdate()` code block.

This script just shows you how to use this construct to build and populate a model from scratch. Appending content to a model is done by, sequentially:

-   Creating spot objects. You have to provide their x, y, z location, as well as a radius and a quality value for each. At this stage, you don't provide at what frame (or time) they belong.
-   This is done by adding the spot to the model, using `model.addSpotTo(Spot, frame)`, frame being a positive integer number.
-   Then you create a link, or an edge as it is called in TrackMate, between two spots. You have to provide the link cost: `model.addEdge(Spot1, Spot2, cost)`.

Spot quality and link cost are typically useful to quantify automatic spot detection and linking. We typically use negative values for these two numbers when doing manual edits.

The script below does this: ![](/media/plugins/trackmate/trackmate-animatedname.gif)

    import ij.gui.NewImage as NewImage
    import fiji.plugin.trackmate.Settings as Settings
    import fiji.plugin.trackmate.Model as Model
    import fiji.plugin.trackmate.Logger as Logger
    import fiji.plugin.trackmate.Spot as Spot
    import fiji.plugin.trackmate.SelectionModel as SelectionModel
    import fiji.plugin.trackmate.TrackMate as TrackMate
    import fiji.plugin.trackmate.visualization.hyperstack.HyperStackDisplayer as HyperStackDisplayer
    import fiji.plugin.trackmate.visualization.trackscheme.TrackScheme as TrackScheme
    import fiji.plugin.trackmate.visualization.PerTrackFeatureColorGenerator as PerTrackFeatureColorGenerator
    import fiji.plugin.trackmate.features.ModelFeatureUpdater as ModelFeatureUpdater
    import fiji.plugin.trackmate.features.track.TrackIndexAnalyzer as TrackIndexAnalyzer
    import ij.plugin.Animator as Animator
    import math

    # We just need a model for this script. Nothing else, since
    # we will do everything manually.
    model = Model()
    model.setLogger(Logger.IJ_LOGGER)

    # Well actually, we still need a bit:
    # We want to color-code the tracks by their feature, for instance
    # with the track index. But for this, we need to compute the
    # features themselves.
    #
    # Manuall, this is done by declaring what features interest you
    # in a settings object, and creating a ModelFeatureUpdater that
    # will listen to changes in the model, and compute the feautures
    # on the fly.
    settings = Settings()
    settings.addTrackAnalyzer( TrackIndexAnalyzer() )
    # If you want more, add more analyzers.

    # The object in charge of keeping the numerical features
    # up to date:
    ModelFeatureUpdater( model, settings )
    # Nothing more to do. When the model changes, this guy will be notified and
    # recalculate all the features you declared in the settings object.



    # Every manual edit to the model must be made
    # between a model.beginUpdate() and a model.endUpdate()
    # call, otherwise you will mess with the event signalling
    # and feature calculation.
    model.beginUpdate()

    # 1.

    s1 = None
    for t in range(0, 5):
        x = 10 + t * 10
        if s1 is None:

            # When you create a spot, you always have to specify its x, y, z
            # coordinates (even if z=0 in 2D images), AND its radius, AND its
            # quality. We enforce these 5 values so as to avoid any bad surprise
            # in other TrackMate component.
            # Typically, we use negative quality values to tag spot created
            # manually.
            s1 = Spot(x, 10, 0, 1, -1)
            model.addSpotTo(s1, t)
            continue


        s2 = Spot(x, 10, 0, 1, -1)
        model.addSpotTo(s2, t)
        # You need to specify an edge cost for the link you create between two spots.
        # Again, we use negative costs to tag edges created manually.
        model.addEdge(s1, s2, -1)
        s1 = s2

    # So that's how you manually build a model from scratch.
    # The next lines just do more of this, to build something enjoyable.

    middle = s2
    s1 = s2
    for t in range(0, 4):
        x = 60 + t * 10
        s2 = Spot(x, 10, 0, 1, -1)
        model.addSpotTo(s2, t + 5)
        model.addEdge(s1, s2, -1)
        s1 = s2

    s1 = middle
    for t in range(0, 16):
        y = 20 + t * 6
        s2 = Spot(50, y, 0, 1, -1)
        model.addSpotTo(s2, t + 5)
        model.addEdge(s1, s2, -1)
        s1 = s2


    # 2.

    s1 = None
    for t in range(0, 21):
        if s1 is None:
            s1 = Spot(110, 10, 0, 1, -1)
            model.addSpotTo(s1, t)
            start = s1
            continue

        y = 10 + t * 5
        s2 = Spot(110, y, 0, 1, -1)
        model.addSpotTo(s2, t)
        model.addEdge(s1, s2, -1)

        if t == 10:
            middle = s2

        s1 = s2

    s1 = start
    for t in range(1, 20):
        theta = math.pi - t * math.pi / 20
        x = 110 + 40 * math.sin(theta)
        y = 35 + 25 * math.cos(theta)
        s2 = Spot(x, y, 0, 1, -1)   
        model.addSpotTo(s2, t)
        model.addEdge(s1, s2, -1)
        s1 = s2

    s1 = middle
    for t in range(1, 11):
        x = 110 + t * 5
        y = 60 + t * 5
        s2 = Spot(x, y, 0, 1, -1)   
        model.addSpotTo(s2, t+10)
        model.addEdge(s1, s2, -1)
        s1 = s2


    # 3.

    s1 = None
    for t in range(0, 21):
        if s1 is None:
            s1 = Spot(170, 110, 0, 1, -1)
            model.addSpotTo(s1, t)
            continue

        x = 170 + t * 4
        if t < 10:
            y = 110 - t * 10
        else:
            y = 10 + (t-10) * 10
        s2 = Spot(x, y, 0, 1, -1)   
        model.addSpotTo(s2, t)
        model.addEdge(s1, s2, -1)
        s1 = s2

        if t == 5:
            start = s2
        if t == 15:
            end = s2

    s1 = start
    for t in range(6, 15):
        x = 194 + (t-5) * 3.5
        s2 = Spot(x, 60, 0, 1, -1)
        model.addSpotTo(s2, t)
        model.addEdge(s1, s2, -1)
        s1 = s2

    model.addEdge(s2, end, -1)


    # 4.
    # A nice partial squircle

    n = 4
    a = 30
    b = 50
    s1 = None
    for t in range(0, 21):
        theta = math.pi/10.0 + t/20.0 * (2 * math.pi - math.pi/5.0)

        ct = math.cos(theta)
        if  ct > 0:
            sgn = +1 # copysign is not available to us :(
        else:
            sgn = -1
        x = 290 + math.pow(math.fabs( ct ) , (2.0/n)) * a * sgn

        st = math.sin(theta)
        if  st > 0:
            sgn = +1
        else:
            sgn = -1
        y = 60 + math.pow(math.fabs( st ) , (2.0/n)) * b * sgn

        s2 = Spot(x, y, 0, 1, -1)
        model.addSpotTo(s2, t)

        if s1 is None:
            s1 = s2
            continue

        model.addEdge(s1, s2, -1)
        s1 = s2


    #5.

    s1 = None
    s3 = None
    for t in range(0, 21):
        x1 = 340
        y1 = 10 + t * 5
        y2 = y1

        s2 = Spot(x1, y1, 0, 1, -1)
        model.addSpotTo(s2, t)

        if t == 10:
            s4 = s2
        else:
            if t < 10:
                x2 = 400 - t *  6
            else:
                x2 = 340 + (t-10) * 6
            s4 = Spot(x2, y1, 0, 1, -1)

        model.addSpotTo(s4, t)

        if s1 is None:
            s1 = s2
            s3 = s4
            continue

        model.addEdge(s1, s2, -1)
        model.addEdge(s3, s4, -1)
        s1 = s2
        s3 = s4


    # 6.
    s1 = None
    s3 = None
    for t in range(0, 6):
        y = 40 - t * 6
        x1 = 450 - t * 8
        x2 = 450 + t * 8

        s2 = Spot(x1, y, 0, 1, -1)
        model.addSpotTo(s2, t)
        if s1 is None:
            s1 = s2
            s3 = s2
            continue

        s4 = Spot(x2, y, 0, 1, -1)
        model.addSpotTo(s4, t)
        model.addEdge(s1, s2, -1)
        model.addEdge(s3, s4, -1)
        s1 = s2
        s3 = s4

    # If, at this stage, you start thinking I have too much free time,
    # then I would welcome your application as a babysitter to watch
    # over the kids while I go out and spend a nice evening with my
    # wife.

    for t in range(6, 21):
        x1 = 410
        x2 = 490
        y = 10 + (t-6) * 7
        s2 = Spot(x1, y, 0, 1, -1)
        s4 = Spot(x2, y, 0, 1, -1)
        model.addSpotTo(s2, t)
        model.addSpotTo(s4, t)
        model.addEdge(s1, s2, -1)
        model.addEdge(s3, s4, -1)
        s1 = s2
        s3 = s4


    # 7.
    # The power of copy-paste

    s1 = None
    for t in range(0, 21):
        if s1 is None:
            s1 = Spot(510, 110, 0, 1, -1)
            model.addSpotTo(s1, t)
            continue

        x = 510+ t * 4
        if t < 10:
            y = 110 - t * 10
        else:
            y = 10 + (t-10) * 10
        s2 = Spot(x, y, 0, 1, -1)   
        model.addSpotTo(s2, t)
        model.addEdge(s1, s2, -1)
        s1 = s2

        if t == 5:
            start = s2
        if t == 15:
            end = s2

    s1 = start
    for t in range(6, 15):
        x = 534 + (t-5) * 3.5
        s2 = Spot(x, 60, 0, 1, -1)
        model.addSpotTo(s2, t)
        model.addEdge(s1, s2, -1)
        s1 = s2

    model.addEdge(s2, end, -1)


    # 8.
    # The power of copy-paste
    # At this stage I wish Nick named this plugin Tatata.

    s1 = None
    for t in range(0, 5):
        x = 590 + t * 10
        if s1 is None:
            s1 = Spot(x, 10, 0, 1, -1)
            model.addSpotTo(s1, t)
            continue


        s2 = Spot(x, 10, 0, 1, -1)
        model.addSpotTo(s2, t)
        model.addEdge(s1, s2, -1)
        s1 = s2


    middle = s2

    s1 = s2
    for t in range(0, 4):
        x = 640 + t * 10
        s2 = Spot(x, 10, 0, 1, -1)
        model.addSpotTo(s2, t + 5)
        model.addEdge(s1, s2, -1)
        s1 = s2

    s1 = middle
    for t in range(0, 16):
        y = 20 + t * 6
        s2 = Spot(630, y, 0, 1, -1)
        model.addSpotTo(s2, t + 5)
        model.addEdge(s1, s2, -1)
        s1 = s2


    # 9.
    s1 = None
    for t in range(0, 6):
        y = 60
        x = 720 - t * 8
        s2 = Spot(x, y, 0, 1, -1)
        model.addSpotTo(s2, t)

        if s1 is None:
            s1 = s2
            continue

        model.addEdge(s1, s2, -1)
        s1 = s2

    s3 = s1
    for t in range(6, 11):
        x = 680
        y1 = 60 + (t-5) * 10
        y2 = 60 - (t-5) * 10
        s2 = Spot(x, y1, 0, 1, -1)
        s4 = Spot(x, y2, 0, 1, -1)
        model.addSpotTo(s2, t)
        model.addSpotTo(s4, t)
        model.addEdge(s1, s2, -1)
        model.addEdge(s3, s4, -1)
        s1 = s2
        s3 = s4

    for t in range(11, 21):
        y1 = 110
        y2 = 10
        x = 680 + (t-10) * 5
        s2 = Spot(x, y1, 0, 1, -1)
        s4 = Spot(x, y2, 0, 1, -1)
        model.addSpotTo(s2, t)
        model.addSpotTo(s4, t)
        model.addEdge(s1, s2, -1)
        model.addEdge(s3, s4, -1)
        s1 = s2
        s3 = s4

    # Change the radius of the last one, so that we create some blank
    # space around the model for display.
    s2.putFeature('RADIUS', 20)


    # Commit all of this.
    model.endUpdate()
    # This actually triggers the features to be recalculated.


    # Prepare display.
    sm = SelectionModel(model)
    color = PerTrackFeatureColorGenerator(model, 'TRACK_INDEX')
    # The last line does not work if you did not compute the 'TRACK_INDEX'
    # feature earlier.


    # The TrackScheme view is a bit hard to interpret.
    trackscheme = TrackScheme(model, sm)
    trackscheme.setDisplaySettings('TrackColoring', color)
    trackscheme.render()

    # You can create an hyperstack viewer without specifying any ImagePlus.
    # It will then create a dummy one tuned to display the model content.
    view = HyperStackDisplayer(model, sm)
    # Display tracks as comets
    view.setDisplaySettings('TrackDisplaymode', 1)
    view.setDisplaySettings('TrackDisplayDepth', 20)
    view.setDisplaySettings('TrackColoring', color)
    view.render()

    # Animate it a bit
    imp = view.getImp()
    imp.getCalibration().fps = 30
    Animator().run('start')

## Calling TrackMate with multi-channel analyzer

TrackMate allows for the addition of jar files that contain extra TrackMate modules. The [multi-channel spot mean intensity analyzer](/plugins/trackmate#downloadable-jars) is such a module.

As any other module it can be used in a script, provided the jar file is in the plugins or jars folder of Fiji:

{% include code org='fiji' repo='TrackMate' branch='master' path='scripts/CallTrackMateMultiChannel.py' %}

## Making TrackMate macro recordable with a 64-line script

Contributed by {% include person id='imagejan' %} during a NEUBIAS course. Quoting from Jan:

> "The macro language is too limited to work with such awesome things as TrackMate, but that you can do everything with a more powerful scripting language. So when using a 64-line script to call it, it actually is macro recordable."

{% include code org='fiji' repo='TrackMate' branch='master' path='scripts/Run_TrackMate_Headless.groovy' %}

## Add 3D maximas in the ROI Manager using TrackMate

Using the 3D spots finder of TrackMate, it is possible to add the maximas to the ROI Manager with a simple Jython code:

```python
# @ImagePlus imp

# Imports
from fiji.plugin.trackmate.detection import LogDetector
from net.imglib2.img.display.imagej import ImageJFunctions

from ij.plugin.frame import RoiManager
from ij.gui import PointRoi

# Set the parameters for LogDetector
img = ImageJFunctions.wrap(imp)
interval = img
cal = imp.getCalibration()
# Get the calibration from the metadata if exists
calibration = [cal.pixelWidth, cal.pixelHeight, cal.pixelDepth]

# Values to enter based on the TrackMate GUI
radius = 5  # the radius is half the diameter
threshold = 1050
doSubpixel = True
doMedian = True


# Setup spot detector (see http://javadoc.imagej.net/Fiji/fiji/plugin/trackmate/detection/LogDetector.html)
#
# public LogDetector(RandomAccessible<T> img,
#            Interval interval,
#            double[] calibration,
#            double radius,
#            double threshold,
#            boolean doSubPixelLocalization,
#            boolean doMedianFilter)

detector = LogDetector(img, interval, calibration, radius, threshold, doSubpixel, doMedian)

# Start processing and display the results
if detector.process():
    # Get the list of peaks found
    peaks = detector.getResult()
    print str(len(peaks)), "peaks were found."

    # Add points to ROI manager
    rm = RoiManager.getInstance()
    if not rm:
        rm = RoiManager()

    # Loop through all the peak that were found
    for peak in peaks:
        # Print the current coordinates
        print peak.getDoublePosition(0), peak.getDoublePosition(1), peak.getDoublePosition(2)
        # Add the current peak to the Roi manager
        roi = PointRoi(peak.getDoublePosition(0) / cal.pixelWidth, peak.getDoublePosition(1) / cal.pixelHeight)
        # Set the Z position of the peak otherwise the peaks are all set on the same slice
        roi.setPosition(int(round(peak.getDoublePosition(2) / cal.pixelDepth))+1)
        rm.addRoi(roi)
    # Show all ROIs on the image
    rm.runCommand(imp, "Show All")

else:
    print "The detector could not process the data."
```

## Tracking spots that are taken from the ROI manager.

You have to start from a 2D+T image (nothing else) and a results table that contains at least the center of mass XM, YM, the slice and the Area for the cells in the movie. The results table is typically generated from the ROI manager, that would contain the results of the particle analyzer.

So an ideal starting situation would like this:

<img src="/media/plugins/trackmate/trackmatescriptbeforecapture.png" width="600"/>

this script will generate the following tracks:

https://aws1.discourse-cdn.com/business4/uploads/imagej/original/3X/c/3/c3cee76938213f8d3e43ae1ca4cfbe3e4453546e.gif

Cool no? The output can be controlled via a TrackMate GUI that will be shown upon running the script. Showing the GUI might not be desirable in batch mode, but from the GUI you can save your data, export to IJ tables and save to CSV, export a movie etc.

The script also offers to color the ROIs by track ID, if you have the ROI manager that was used to create the results table. It looks like this:

https://aws1.discourse-cdn.com/business4/uploads/imagej/original/3X/2/3/237d4f70b01b9d4fa596f89adea5c10fc481fbc0.gif

    import sys
    from math import pi
    from math import sqrt
    from random import shuffle

    from java.awt import Color

    from ij import WindowManager
    from ij.measure import ResultsTable
    from ij.plugin.frame import RoiManager
    from ij import IJ

    from fiji.plugin.trackmate import Logger
    from fiji.plugin.trackmate import Model
    from fiji.plugin.trackmate import SelectionModel
    from fiji.plugin.trackmate import Settings
    from fiji.plugin.trackmate import Spot
    from fiji.plugin.trackmate import SpotCollection
    from fiji.plugin.trackmate import TrackMate
    from fiji.plugin.trackmate.detection import ManualDetectorFactory
    from fiji.plugin.trackmate.tracking import LAPUtils
    from fiji.plugin.trackmate.providers import SpotAnalyzerProvider
    from fiji.plugin.trackmate.providers import EdgeAnalyzerProvider
    from fiji.plugin.trackmate.providers import TrackAnalyzerProvider
    from fiji.plugin.trackmate.tracking.sparselap import SparseLAPTrackerFactory
    from fiji.plugin.trackmate.visualization.hyperstack import HyperStackDisplayer
    from fiji.plugin.trackmate.gui import TrackMateGUIController
    from org.jfree.chart.renderer.InterpolatePaintScale import Jet






    def spots_from_results_table( results_table, frame_interval ):
        **
        Creates a spot collection from a results table in ImageJ.
        Requires the current results table, in which the results from
        particle analysis should be. We need at least the center
        of mass, the area and the slice to be specified there.
        We also query the frame interval to properly generate the
        POSITION_T spot feature.
        **

        frames = results_table.getColumnAsDoubles( results_table.getColumnIndex( 'Slice' ) )
        xs = results_table.getColumnAsDoubles( results_table.getColumnIndex( 'XM' ) )
        ys = results_table.getColumnAsDoubles( results_table.getColumnIndex( 'YM' ) )
        z = 0.
        # Get radiuses from area.
        areas = results_table.getColumnAsDoubles( results_table.getColumnIndex( 'Area' ) )
        spots = SpotCollection()

        for i in range( len( xs ) ):
            x = xs[ i ]
            y = ys[ i ]
            frame = frames[ i ]
            area = areas[ i ]
            t = ( frame - 1 ) * frame_interval
            radius = sqrt( area / pi )
            quality = i # Store the line index, to later retrieve the ROI.
            spot = Spot( x, y, z, radius, quality )
            spot.putFeature( 'POSITION_T', t )
            spots.add( spot, int( frame - 1 ) )

        return spots


    def create_trackmate( imp, results_table ):
        **
        Creates a TrackMate instance configured to operated on the specified
        ImagePlus imp with cell analysis stored in the specified ResultsTable
        results_table.
        **

        cal = imp.getCalibration()

        # TrackMate.

        # Model.
        model = Model()
        model.setLogger( Logger.IJ_LOGGER )
        model.setPhysicalUnits( cal.getUnit(), cal.getTimeUnit() )

        # Settings.
        settings = Settings()
        settings.setFrom( imp )

        # Create the TrackMate instance.
        trackmate = TrackMate( model, settings )

        # Add ALL the feature analyzers known to TrackMate, via
        # providers.
        # They offer automatic analyzer detection, so all the
        # available feature analyzers will be added.
        # Some won't make sense on the binary image (e.g. contrast)
        # but nevermind.

        spotAnalyzerProvider = SpotAnalyzerProvider()
        for key in spotAnalyzerProvider.getKeys():
            print( key )
            settings.addSpotAnalyzerFactory( spotAnalyzerProvider.getFactory( key ) )

        edgeAnalyzerProvider = EdgeAnalyzerProvider()
        for  key in edgeAnalyzerProvider.getKeys():
            print( key )
            settings.addEdgeAnalyzer( edgeAnalyzerProvider.getFactory( key ) )

        trackAnalyzerProvider = TrackAnalyzerProvider()
        for key in trackAnalyzerProvider.getKeys():
            print( key )
            settings.addTrackAnalyzer( trackAnalyzerProvider.getFactory( key ) )

        trackmate.getModel().getLogger().log( settings.toStringFeatureAnalyzersInfo() )
        trackmate.computeSpotFeatures( True )
        trackmate.computeEdgeFeatures( True )
        trackmate.computeTrackFeatures( True )

        # Skip detection and get spots from results table.
        spots = spots_from_results_table( results_table, cal.frameInterval )
        model.setSpots( spots, False )

        # Configure detector. We put nothing here, since we already have the spots
        # from previous step.
        settings.detectorFactory = ManualDetectorFactory()
        settings.detectorSettings = {}
        settings.detectorSettings[ 'RADIUS' ] = 1.

        # Configure tracker
        settings.trackerFactory = SparseLAPTrackerFactory()
        settings.trackerSettings = LAPUtils.getDefaultLAPSettingsMap()
        settings.trackerSettings[ 'LINKING_MAX_DISTANCE' ]      = 20.0
        settings.trackerSettings[ 'GAP_CLOSING_MAX_DISTANCE' ]  = 20.0
        settings.trackerSettings[ 'MAX_FRAME_GAP' ]             = 3

        settings.initialSpotFilterValue = -1.

        return trackmate



    def process( trackmate ):
        **
        Execute the full process BUT for the detection step.
        **
        # Check settings.
        ok = trackmate.checkInput()
        # Initial filtering
        print( 'Spot initial filtering' )
        ok = ok and trackmate.execInitialSpotFiltering()
        # Compute spot features.
        print( 'Computing spot features' )
        ok = ok and trackmate.computeSpotFeatures( True )
        # Filter spots.
        print( 'Filtering spots' )
        ok = ok and trackmate.execSpotFiltering( True )
        # Track spots.
        print( 'Tracking' )
        ok = ok and trackmate.execTracking()
        # Compute track features.
        print( 'Computing track features' )
        ok = ok and trackmate.computeTrackFeatures( True )
        # Filter tracks.
        print( 'Filtering tracks' )
        ok = ok and trackmate.execTrackFiltering( True )
        # Compute edge features.
        print( 'Computing link features' )
        ok = ok and trackmate.computeEdgeFeatures( True )

        return ok


    def display_results_in_GUI( trackmate ):
        **
        Creates and show a TrackMate GUI to configure the display
        of the results.

        This might not always be desriable in e.g. batch mode, but
        this allows to save the data, export statistics in IJ tables then
        save them to CSV, export results to AVI etc...
        **

        gui = TrackMateGUIController( trackmate )

        # Link displayer and GUI.

        model = trackmate.getModel()
        selectionModel = SelectionModel( model)
        displayer = HyperStackDisplayer( model, selectionModel, imp )
        gui.getGuimodel().addView( displayer )
        displaySettings = gui.getGuimodel().getDisplaySettings()
        for key in displaySettings.keySet():
            displayer.setDisplaySettings( key, displaySettings.get( key ) )
        displayer.render()
        displayer.refresh()

        gui.setGUIStateString( 'ConfigureViews' )



    def color_rois_by_track( trackmate, rm, results_table ):
        **
        Colors the ROIs stored in the specified ROIManager rm using a color
        determined by the track ID they have.

        We retrieve the IJ ROI that matches the TrackMate Spot because in the
        latter we stored the index of the spot in the quality feature. This
        is a hack of course. On top of that, it supposes that the index of the
        ROI in the ROIManager corresponds to the line in the ResultsTable it
        generated. So any changes to the ROIManager or the ResultsTable is
        likely to break things.
        **

        rt = ResultsTable.getResultsTable()
        model = trackmate.getModel()
        track_colors = {}
        track_idds = {}
        track_indices = []

        for i in model.getTrackModel().trackIDs( True ):
            track_indices.append( i )
        shuffle( track_indices )

        index = 0
        for track_id in track_indices:
            color = Jet.getPaint( float(index) / ( len( track_indices) - 1 ) )
            track_colors[ track_id ] = color
            index = index + 1


        spots = model.getSpots()
        spotN=0;
        for spot in spots.iterable( True ):
            q = spot.getFeature( 'QUALITY' ) # Stored the ROI id.
            roi_id = int( q )
            roi = rm.getRoi( roi_id )

            # Get track id.
            track_id = model.getTrackModel().trackIDOf( spot )
            if track_id is None:
                color = Color.GRAY
            else:
                color = track_colors[ track_id ]

            roi.setFillColor( color )

            roiF=roi.getPosition()
            #rename ROI
            if track_id is None:
                rm.rename(roi_id,"trNaN_f"+str(roiF)+"_"+roi.getName())
            else:
                rm.rename(roi_id,"tr"+str(track_indices[track_id])+"_f"+str(roiF)+"_"+roi.getName())
            #addResults table entry
            if track_id is None:
                rt.setValue("TrackN",roi_id, "NaN")
            else:
                rt.setValue("TrackN",roi_id, track_indices[track_id])

            spotN=spotN+1

        results_table.updateResults()
        rt.show("Results")

    #------------------------------
    #           MAIN
    #------------------------------

    # Get current image.
    imp = WindowManager.getCurrentImage()

    # Remove overlay if any.
    imp.setOverlay( None )

    # Get results table.
    results_table = ResultsTable.getResultsTable()

    # Create TrackMate instance.
    trackmate = create_trackmate( imp, results_table )

    #-----------------------
    # Process.
    #-----------------------

    ok = process( trackmate )
    if not ok:
        sys.exit(str(trackmate.getErrorMessage()))

    #-----------------------
    # Display results.
    #-----------------------

    # Create the GUI and let it control display of results.
    display_results_in_GUI( trackmate )

    # Color ROI by track ID!
    rm = RoiManager.getInstance()
    color_rois_by_track( trackmate, rm, results_table )
