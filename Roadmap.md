# Goals of dicompyler #

dicompyler seeks to be an open source tool for radiation therapy based on the DICOM standard using Python based technologies. To be as useful as possible, a plugin-style architecture will be adopted to enable extensibility.

If you are interested in open source, Python-based tools for radiation therapy QA and dosimetry, please check out the [RadPy](http://code.google.com/p/radpy/) project.

## Specific goals ##

  * Read and write DICOM files.
  * Read and write DICOM-RT files, as well as extensions, such as DICOM-RT-Ion.
  * Display image, contour, DVH, and isodose data.
  * Contour/re-contour volumes.
  * Enable 3rd party plugins, such as dose-calculation algorithms.
  * Many other treatment planning and treatment comparison related capabilities.

# Timeline #

Keeping in mind that roadmaps and timelines are always speculative...

**As of December 31st, 2011**

## Version 0.1 ##
**Released**: February 20th, 2010

**Features**:
  * Import DICOM-RT
  * Display list of structures
  * Display associated cDVH's (with adjustable constraints)

## Version 0.2 ##
**Released**: July 17th, 2010

**Features**:
  * New tabbed interface
  * Initial plugin architecture in place
  * Example plugins - (**[PluginDevelopmentGuide](http://code.google.com/p/dicompyler/wiki/PluginDevelopmentGuide)**)
    * Display tree of all DICOM data
    * Display all DVH's on same plot (selectively)
    * Display image data with structures and dose
    * DICOM anonymization

## Version 0.3 ##
**Released**: October 5th, 2010

**Features**:
  * 2D image viewer
    * Added Mousewheel image navigation
    * Added Zoom controls via toolbar and +/- keys
    * Added Panning of image via left mouse dragging
    * Added Window & level control via right mouse dragging
  * DVH viewer
    * Simplified and streamlined the DVH constraint interface
  * General
    * Many bugfixes
    * Plugins now can have toolbar buttons and will respond to focus events
  * See the [Release Notes](http://code.google.com/p/dicompyler/wiki/ReleaseNotes) for more detailed information

## Version 0.4 ##
**Released**: Test Version 0.4a2 - May 19th, 2011

**Features**:
  * Import Differential DVH
  * Generation of DVH from DICOM RT Dose matrix
  * Preferences for each plugin

## Version 0.4.1 ##
**Released**: December 31st, 2011

**Features**:
  * Refactored into a [Python package](http://pypi.python.org/pypi/dicompyler)
  * Error reporter for easier submission of bug reports from binary versions
  * Logging for dicompyler and 3rd-party plugins
  * Support volumetric image series other than CT (i.e. PET/MR)
  * Support DICOM-RT Ion Plans

## Version 0.4.2 ##
**Release**: July 15th, 2014 (**[Download now](http://code.google.com/p/dicompyler/downloads/list)**)

**Features**:
  * Added Quick DICOM Import plugin
  * Plugins can now access the 2D View drawing canvas
  * Advanced Plugin Manager
    * Can enable/disable plugins
  * DICOM data can now be loaded via command line argument or dragging and dropping a folder on the dicompyler icon on the Windows version
  * Added persistence of the window size and position
  * Many bug fixes

## Version 0.5 ##
**Release**: Future, TBD

**Features**:
  * Auto-updater for binary versions
  * Auto-updater for binary versions
  * Advanced Plugin Manager
    * Can enable/disable plugins
    * Download plugins from dicompyler plugin repository
  * Structure interpolation
  * 3-pane 2D Image viewer (transverse, sagittal, coronal)
  * Editing of all DICOM fields
  * Compare DVH's from two separate RT plans
  * Display of dose on image (colorwash)

## Version 0.6 ##
**Release**: Future, TBD

**Features**:
  * Performance boosts
  * Code refactoring
  * PEP-8 compliance
  * Documentation overhaul

## Version ?? ##
**Release**: Future, TBD

**Features**:
  * Python 3.x compatibility