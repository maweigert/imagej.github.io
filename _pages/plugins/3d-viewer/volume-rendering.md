---
mediawiki: 3D_Viewer:_Volume_Rendering
title: 3D Viewer › Volume Rendering
---

(Return to the [Developer Documentation](/plugins/3d-viewer/developer-documentation) page)  
(Return to the main [3D\_Viewer](/plugins/3d-viewer) page)

## How to work with volume renderings

You can download example source code for this HowTo [here](/plugins/3d-viewer/example-code).

Before reading this HowTo, it may be helpful to read [The relation between Content and Universe](/plugins/3d-viewer/content-structure).

It is possible to edit a volume rendering. `VoltexGroup` provides a method `fillRoi()`, which takes as input parameters a ROI and a fill value. The ROI (in canvas coordinates) is projected onto the volume, and the covered part of the volume is filled with the specified fill value.

Because `fillRoi()` is a method which only applies to volume renderings (orthoslices being an exception), this method is not located in Content.java, but in VoltexGroup.java.

`VoltexGroup` is a subclass of `ContentNode`, and can be retrieved from a `Content` (one which is displayed as a volume rendering) with `getContent()`:

        // Add the image as a volume
        Content c = univ.addVoltex(imp);


        // Retrieve the VoltexGroup
        VoltexGroup voltex = (VoltexGroup)c.getContent();

        // Define a ROI
        Roi roi = new OvalRoi(240, 220, 70, 50);

        // Define a fill color
        byte fillValue = (byte)100;

        // Fill the part of the volume which results from the
        // projection of the polygon onto the volume:
        voltex.fillRoi(univ.getCanvas(), roi, fillValue);

One thing worth to keep in mind is that also the original image is changed, and can in this form be saved, if desired.
