---
title: Roi 1-click tools
name: Roi 1-click tools
description: Annotation with ROIs of predefined dimensions
categories: [Image Annotation]

source-url: https://github.com/LauLauThom/Fiji-RoiClickTools
release-date : 2019

license-url: /licensing/bsd
license-label: BSD-2

team-leads: Laurent Thomas | mailto:l.thomas@acquifer.de
---

# New versions

**18/06/2020**  
- Fix ROI slice-position that was not saved previously.  
This requires ImageJ &gt;= 1.52r, otherwise a message is shown when annotating stacks.

**29/05/2020**  
- Add ROI group to click-tools settings if ImageJ &gt;= 1.52t

**22/05/2020**  
- Add custom ROI buttons (define from active ROI)  
- Add help button in toolbar

**15/05/2020**  
- Save settings in memory for next run  
- Show new default group in ImageJ status bar upon pressing numerical keypad shortcuts

**08/04/2020**  
- Add numerical keyboard shortcut, such that the 0 to 9 keys of the keypad on the right of the keyboard (not the top row) can be used to set the current roi group

# Installation

In Fiji, activate the **ROI 1-click tools** update site.  
In ImageJ, copy the file *Roi 1-Click Tools.ijm* to *ImageJ\\macros\\toolsets*

# Documentation

See the [Readme](https://github.com/LauLauThom/Fiji-RoiClickTools) of the GitHub repo.

# Citation

{% include citation doi='10.17912/micropub.biology.000215' %}

# Video tutorial

{% include video platform="youtube" id="PLbBgXlYof3_aEXBmXkkuLyQ78aVj_xdan" %} 
