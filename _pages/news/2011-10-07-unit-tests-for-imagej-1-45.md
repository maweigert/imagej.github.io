---
title: 2011-10-07 - Unit tests for ImageJ 1.45
---

When we launched the [ImageJ2](/software/imagej2) project, we began by writing many unit tests to protect [ImageJ 1.x](/software/imagej1) from regression bugs. At this writing, there are unit tests in place for 50 core IJ1 classes, intended to detect bugs introduced during ImageJ development. These tests are located in the {% include github org='imagej' repo='ij1-tests' label='ij1-tests repository' %}.

Originally, the tests were designed to run against a modified version of IJ1 that we were developing to facilitate integration with IJ2. However, as development continued, we settled on a design that achieves maximum [compatibility](/libs/imagej-legacy) by embedding IJ1 as-is within IJ2, enabling IJ1 to continue developing in parallel with IJ2. We eliminated the modified version of IJ1, with only the unit tests being retained. Unfortunately, we did not finish fully transitioning the tests, and they were not compiling against stock versions of IJ1.

As of now, we have fixed up the tests to compile ([r4092](https://github.com/imagej/imagej/commit/9fc9ac6599c279bc83eb0a62d922f34517c47e37)) and run ([r4096](https://github.com/imagej/imagej/commit/b51494bb19c094b4430d3936a5e30383c722b35a), [r4098](https://github.com/imagej/imagej/commit/d1d4ffd94096a0843e751533d909513faaccb7c3)) against stock versions of ImageJ v1.44 and later, and posted [instructions for running them](/develop/ij1-unit-tests).

We then ran the tests against all the 1.45 development versions. The results are below.

Over time, a small but increasing number of tests are failing, which indicates changing behavior creeping into the code. Of course, sometimes changing behavior is good, such as when bugs are fixed. But sometimes these sorts of changes indicate regression bugs being introduced. The only way to know for sure is to examine each failure on an individual basis. For each one, either the test needs to be updated (in the case of a bugfix causing the test to fail when testing for the previous buggy behavior), or the code needs to be fixed to preserve the prior behavior (in the case of a regression bug having been introduced).

Once all the tests pass, [Jenkins](/develop/jenkins) can run them automatically whenever a new version of ImageJ 1.x is released, and report any failures via email, so that we become aware as early as possible when regression bugs have been introduced.

{::nomarkdown}
<table>
  <thead>
    <tr class="header">
      <th>
        <p>Version(s)</p>
      </th>
      <th>
        <p>Results</p>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p>1.43u</p>
      </td>
      <td>
        <p>DOES NOT COMPILE (version too old)</p>
      </td>
    </tr>
    <tr>
      <td>
        <p>1.44o</p>
      </td>
      <td>
        <p>ALL PASS</p>
      </td>
    </tr>
    <tr>
      <td>
        <p>1.45a</p>
      </td>
      <td>
        <p>ALL PASS</p>
      </td>
    </tr>
    <tr>
      <td>
        <p>1.45b</p>
      </td>
      <td>
        <p>2 FAILURES:</p>
        <ol>
          <li><strong>testFitSplineForStraightening(ij.gui.PolygonRoiTest): array lengths differed, expected.length=4 actual.length=8</strong></li>
          <li><strong>testGetUncalibratedLength(ij.gui.PolygonRoiTest): expected:&lt;1.0&gt; but was:&lt;6.324555320336759&gt;</strong></li>
        </ol>
      </td>
    </tr>
    <tr>
      <td>
        <p>1.45c - 1.45h</p>
      </td>
      <td>
        <p>4 FAILURES:</p>
        <ol>
          <li>testFitSplineForStraightening(ij.gui.PolygonRoiTest): array lengths differed, expected.length=4 actual.length=8</li>
          <li>testGetUncalibratedLength(ij.gui.PolygonRoiTest): expected:&lt;1.0&gt; but was:&lt;6.324555320336759&gt;</li>
          <li><strong>testGetPixels(ij.VirtualStackTest): arrays first differed at element [2]; expected:&lt;0&gt; but was:&lt;120&gt;</strong></li>
          <li><strong>testGetProcessor(ij.VirtualStackTest): arrays first differed at element [2]; expected:&lt;0&gt; but was:&lt;120&gt;</strong></li>
        </ol>
      </td>
    </tr>
    <tr>
      <td>
        <p>1.45i - 1.45n</p>
      </td>
      <td>
        <p>5 FAILURES:</p>
        <ol>
          <li><strong>testDrawPixelsImageProcessor(ij.gui.ArrowTest): expected:&lt;0&gt; but was:&lt;73&gt;</strong></li>
          <li>testFitSplineForStraightening(ij.gui.PolygonRoiTest): array lengths differed, expected.length=4 actual.length=8</li>
          <li>testGetUncalibratedLength(ij.gui.PolygonRoiTest): expected:&lt;1.0&gt; but was:&lt;6.324555320336759&gt;</li>
          <li>testGetPixels(ij.VirtualStackTest): arrays first differed at element [2]; expected:&lt;0&gt; but was:&lt;120&gt;</li>
          <li>testGetProcessor(ij.VirtualStackTest): arrays first differed at element [2]; expected:&lt;0&gt; but was:&lt;120&gt;</li>
        </ol>
      </td>
    </tr>
    <tr>
      <td>
        <p>1.45o - 1.45q</p>
      </td>
      <td>
        <p>7 FAILURES:</p>
        <ol>
          <li>testDrawPixelsImageProcessor(ij.gui.ArrowTest): expected:&lt;0&gt; but was:&lt;73&gt;</li>
          <li>testFitSplineForStraightening(ij.gui.PolygonRoiTest): array lengths differed, expected.length=4 actual.length=8</li>
          <li>testGetUncalibratedLength(ij.gui.PolygonRoiTest): expected:&lt;1.0&gt; but was:&lt;6.324555320336759&gt;</li>
          <li><strong>testGetConvexHull(ij.gui.ShapeRoiTest)</strong></li>
          <li><strong>testGetFeretValues(ij.gui.ShapeRoiTest): arrays first differed at element [0]; expected:&lt;10.44016&gt; but was:&lt;10.44030650891055&gt;</strong></li>
          <li>testGetPixels(ij.VirtualStackTest): arrays first differed at element [2]; expected:&lt;0&gt; but was:&lt;120&gt;</li>
          <li>testGetProcessor(ij.VirtualStackTest): arrays first differed at element [2]; expected:&lt;0&gt; but was:&lt;120&gt;</li>
        </ol>
      </td>
    </tr>
    <tr>
      <td>
        <p>1.45r+</p>
      </td>
      <td>
        <p>See the <a href="/news/2012-03-20-unit-tests-for-imagej-1-46">ImageJ 1.46 unit tests blog post</a></p>
      </td>
    </tr>
  </tbody>
</table>
{:/}

 
