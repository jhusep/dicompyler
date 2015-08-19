#### For detailed change information, check out the [source code](http://code.google.com/p/dicompyler/source/list) for further information. ####

# Version 0.4.2 - Released July 15th, 2014 #

  * New Features
    * Added Quick DICOM Import feature.
    * Added Import plugin capability with toolbar icon support.
    * Plugins can now access the 2D View drawing canvas.
    * Refactored plugin manager to allow enabling/disabling of plugins.
    * DICOM data can now be loaded via command line argument or dragging and dropping a folder on the dicompyler icon on the Windows version.
    * Added persistence of the window size and position.
    * Added support to determine Rx Dose from RT Plan Fractionation information. [Issue 48](http://code.google.com/p/dicompyler/issues/detail?id=48)
    * Display Rx Dose in the DICOM import window before importing. [Issue 48](http://code.google.com/p/dicompyler/issues/detail?id=48)
    * Added support for DICOM files that don't include the StudyInstanceUID tag.
    * Added support for DICOM RT Dose files without dose grid data and/or DVH data. [Issue 35](http://code.google.com/p/dicompyler/issues/detail?id=35)
    * Added support for DICOM RT Dose files that contain dose for an individual beam. [Issue 44](http://code.google.com/p/dicompyler/issues/detail?id=44)

  * Bug Fixes
    * Fixed a bug in the 2D View where the mouse might be clicked too early after opening a patient. [Issue 62](http://code.google.com/p/dicompyler/issues/detail?id=62)
    * Fixed a bug where a DVH may be found without a corresponding structure, rendering the DVH unusable. [Issue 61](http://code.google.com/p/dicompyler/issues/detail?id=61)
    * Fixed a bug where a forward slash might be used to encode the ROI display color in an RT Structure Set. [Issue 64](http://code.google.com/p/dicompyler/issues/detail?id=64)
    * Support encoded character sets in DICOM files. This addresses the UnicodeDecodeError from occurring in the treeview plugin. [Issue 69](http://code.google.com/p/dicompyler/issues/detail?id=69)
    * Implement sensible defaults for demographics in case they don't exist. [Issue 71](http://code.google.com/p/dicompyler/issues/detail?id=71)
    * Fixed a bug where structures may be listed out of order if many structures exist. [Issue 66](http://code.google.com/p/dicompyler/issues/detail?id=66)
    * Fixed a bug in the 2D View where the image number was incorrect when loading a single image.
    * Fixed a bug where it was possible to start multiple directory search threads in the DICOM import window.
    * Fixed a bug where an index might be out of bounds when obtaining a dose grid slice. [Issue 63](http://code.google.com/p/dicompyler/issues/detail?id=63)
    * Fixed a bug where structure set contours may not display if contour data is not exactly coincident with image data. [Issue 77](http://code.google.com/p/dicompyler/issues/detail?id=77)

# Version 0.4.1-1 - Released January 1st, 2012 #

  * General
    * Fixed an important bug in case dicompyler had never been run before on a system due to the logging system not being initialized properly

# Version 0.4.1 - Released December 31st, 2011 #

  * General
    * Added an error reporter for Windows and Mac versions allowing easy error submission
    * Added support for volumetric image series (i.e. CT, MR, PET) - Resolved [Issue 49](http://code.google.com/p/dicompyler/issues/detail?id=49)
    * Added support for RT Ion Plan
    * Improved image sorting significantly which now allows for non-axial orientations and non-parallel image series
    * Implemented console and file logging, along with a menu item to display log files
    * Implemented an preference to enable detailed logging for debugging
    * Implemented searching of DICOM files within subfolders
    * Implemented the ability to specify the user plugins folder in the preferences
    * Added support for RT Dose files that don't reference RT Plans
    * Added support for unrelated RT Dose and RT Structure Sets that share the same frame of reference
  * DVH calculation and viewer
    * Implemented a preference to force recalculation of DVH data regardless of the presence in RT Dose
    * Limited the number of bins in the DVH calculation to 500 Gy due to high doses in brachytherapy plans
    * Automatically calculate DVHs for a structure only if they are not present within the RT Dose file
  * 2D image viewer
    * Improved support for very low/high doses in the main interface and 2D View - Resolved [Issue 52](http://code.google.com/p/dicompyler/issues/detail?id=52)
  * Anonymization
    * Added support for more tags that should be de-identified - Resolved [Issue 45](http://code.google.com/p/dicompyler/issues/detail?id=45)
  * Bug fixes
    * Fixed a bug where multiple series were loaded when a single series was selected due to the same Frame of Reference
    * Fixed a bug if multiple Target Prescriptions exist in a RT Plan file - Resolved [Issue 55](http://code.google.com/p/dicompyler/issues/detail?id=55)
    * Fixed overestimation of volume calculation when using the DVH to calculate volumes - Resolved [Issue 54](http://code.google.com/p/dicompyler/issues/detail?id=54)
    * Fixed a bug when calculating DVHs for structures that are empty
    * Fixed a bug if DVH data has less bins than actually expected
    * Fixed a bug if the structure data was located slice position with a negative value close to zero
    * Fixed a bug regarding dose display if the current image slice is outside of the dose grid range
    * Fixed a bug where the displayed image width does not correspond to the actual image width - Resolved [Issue 47](http://code.google.com/p/dicompyler/issues/detail?id=47)
    * Fixed a bug with isodose rendering by moving the origin by 1 px right and 1 px down
    * Fixed a bug if the dose grid boundaries were located outside of the image grid - Resolved [Issue 46](http://code.google.com/p/dicompyler/issues/detail?id=46)

# Version 0.4a2 - released May 19th, 2011 #

  * General
    * Automatic conversion of Differential DVHs to Cumulative DVHs - Resolved [Issue 18](http://code.google.com/p/dicompyler/issues/detail?id=18)
    * Automatic import of TomoTherapy Prescription Dose - Resolved [Issue 21](http://code.google.com/p/dicompyler/issues/detail?id=21)
    * Preferences dialog for each plugin with data stored in JSON format
    * Support structures that don't have color information - Resolved [Issue 31](http://code.google.com/p/dicompyler/issues/detail?id=31)
    * Added scrollbars to isodose and structure lists - Resolved [Issue 36](http://code.google.com/p/dicompyler/issues/detail?id=36)
    * Added a status bar to the program to show additional information
    * Added Export plugin capability
    * Added independent DVH calculation from RT Dose / Structure Set data - Resolved [Issue 9](http://code.google.com/p/dicompyler/issues/detail?id=9)
  * 2D image viewer
    * Support CT data that has multiple window / level presets
    * Support for non-coincident dose planes - Resolved [Issue 19](http://code.google.com/p/dicompyler/issues/detail?id=19)
    * New isodose generation using matplotlib backend
    * Holes in contours are now properly displayed
    * Support CT data that is missing the SliceLocation tag - Resolved [Issue 34](http://code.google.com/p/dicompyler/issues/detail?id=34)
    * More accurate panning when zoomed during mouse movement - Resolved [Issue 37](http://code.google.com/p/dicompyler/issues/detail?id=37)
    * Support RescaleIntercept & RescaleSlope tags for more accurate window / level values
    * Added DICOM / pixel coordinate display of the mouse cursor in the status bar - Resolved [Issue 38](http://code.google.com/p/dicompyler/issues/detail?id=38)
    * Added image / dose value display of the mouse cursor in the status bar - Resolved [Issue 38](http://code.google.com/p/dicompyler/issues/detail?id=38)
  * Anonymization
    * Now an export plugin, found under the File->Export menu

# Version 0.3 - released October 5th, 2010 #

  * 2D image viewer
    * Added Mousewheel image navigation
    * Added Zoom controls via toolbar and +/- keys
    * Added Panning of image via left mouse dragging
    * Added Window & level control via right mouse dragging
    * Fixed display of structures in the 2D View for feet first patients - Resolved [Issue 13](http://code.google.com/p/dicompyler/issues/detail?id=13)
    * Fixed image sorting based on slice location rather than instance number
    * Fixed sort order with respect to feet first or head first patient setup
  * DVH viewer
    * Simplified and streamlined the DVH constraint interface
    * Fixed a bug regarding formatting of the constraint result values.
  * General
    * New open patient icon
    * Added focus events for notebook tab plugins
    * Added toolbar buttons for plugins
    * Bugfixes regarding notebook tab focus
    * Fixed a bug if there were two image series present in the same study.
      * If one series was selected, all series would be loaded and merged into one.

# Version 0.2 - released July 17th, 2010 #

  * Can now import DICOM CT, DICOM RT structure set, RT dose and RT plan files
  * Extensible plugin system with included plugins:
    * 2D image viewer with dose and structure overlay
    * Dose volume histogram viewer with the ability to analyze DVH parameters
    * DICOM data tree viewer
    * Patient anonymizer
  * New tabbed interface for plugins
  * Tools menu for menu plugins and plugin manager
  * Selective plugin loading so only the plugins that can use the imported data are loaded
  * New structure list with colors
  * New isodose list with colors (with relative and absolute values)
  * Keyboard support for 2D Image view (up/down, page up/page down)
  * Independent volume calculation if DVH not present or Differential DVH
  * Tons of bugfixes

# Initial version 0.1 - released Feb 20th, 2010 #

  * Ability to import DICOM RT structure set, RT dose and RT plan files
  * Can display and evaluate DVH data from DICOM RT dose
  * Can read DICOM files regardless of file extension
  * Includes optional example DICOM data to get started