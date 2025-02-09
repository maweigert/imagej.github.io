---
mediawiki: 3D_Viewer:_Apply_Transformation
title: 3D Viewer › Apply Transformation
---

(Return to the [Developer Documentation](/plugins/3d-viewer/developer-documentation) page)  
(Return to the main [3D\_Viewer](/plugins/3d-viewer) page)

## How to apply a specific transformation to a Content

You can download example source code for this HowTo [here](/plugins/3d-viewer/example-code).

Each Content may be transformed separately. In the application, this is for example done by the user by selecting a content and dragging with the mouse.

Transformations can also be applied very easy through the API. It is important to keep in mind that there are two distinct types of transformations: **Local transformations**, which are individual for each Content, and **global transformations**, which do not apply to individual Contents, but to the view of the universe.

Here, local transformations are discussed. How to change global transformations is shown in a later HowTo.

It is straightforward to apply a transformation to a Content; after creating the universe and adding a Content (see previous HowTo's), the following code shows how to create a transformation representing a 45 degree rotation around the Y axis:

        // Add the image as an isosurface
        Content c = univ.addVoltex(imp);


        // Create a new Transform3D object
        Transform3D t3d = new Transform3D();

        // Make it a 45 degree rotation around the local y-axis
        t3d.rotY(45 * Math.PI / 180);

        // Apply the transformation to the Content. This concatenates
        // the previous present transformation with the specified one
        c.applyTransform(t3d);

`applyTransform()` concatenates a transformation with a previously existing transformation. So if the call to `applyTransformation()` in the above code is repeated, it results in an overall 90 degree rotation.

There exists another method, `setTransform()`, which does not concatenate transformations, but sets it fixed to the specified one:

        // setTransform() does not concatenate, but sets the specified
        // transformation:
        c.setTransform(t3d);

To reset the transformation of a Content to its initial transformation, which is just the identity, do the following:

        // reset the transformation to the identity
        t3d.setIdentity();
        c.setTransform(t3d);

There exists another method, `lock()`, which can be called for a Content and prevents further transformations.

### Important methods of Content.java, regarding transformation

        public void toggleLock();

        public void setLocked(boolean b);
        
        public void applyTransform(Transform3D transform);
        
        public void setTransform(Transform3D transform);
