# DICOM Compatibility Guide #

In an ideal world, files based on a standard would work perfectly no matter who is the author or generator. Unfortunately, this is not the case with DICOM. Although the DICOM standard is published and freely available, it is up to each vendor to implement the minimal and required objects. However, it is our goal with dicompyler to ensure maximal compatibility with all DICOM data. Therefore, we strive to test with as many different kinds of DICOM data as possible.

If dicompyler cannot read a file, please check this guide first to see if it has been tested. If it is not on this list, either:
  1. Has not been tested
  1. Has not been implemented yet, but is in the process

If you see a system that has not been tested, please let us know and we would be glad to accept any anonymized data for further testing and implementation.

Over the course of dicompyler development, we have noticed some peculiarities with specific DICOM files that will be documented here.

**This guide serves to document what has been tested so far with the current release of dicompyler and will be updated as frequently as possible.**

_Note: The names of the companies and their products listed below are registered trademarks of the respective companies and are only listed here as a guide. This guide also does not denote any official support or endorsement from any company._

## Accuray CyberKnife ##

DICOM Files tested from version 3.3.0
  * CT Images, RT Structures, RT Dose
  * Cumulative DVH with **non** 1 cGy bins
  * Does **NOT** generate RT Plan - **must enter Rx Dose manually**
  * Plugins tested:
    * 2D View, DICOM Tree, Anonymizer - work fine ✔
    * DVH works for structures that have not been calculated, otherwise dose scaling is incorrect for DVH

## CMS XiO ##

DICOM Files tested from version 4.33\_02
  * CT Images, RT Structures, RT Plan, RT Dose
  * No DVH included with RT Dose however DVHs are automatically calculated from the RT Dose directly ([Issue 9](https://code.google.com/p/dicompyler/issues/detail?id=9))
  * Does **NOT** include Rx Dose with RT Plan - **must enter manually**
  * Plugins tested:
    * DICOM Tree, DVH, Anonymizer - work fine ✔
    * 2D View - works fine ✔
      * Works with CT Images and RT Structures
      * RT Dose data is correctly superimposed on the CT Images ([Issue 19](https://code.google.com/p/dicompyler/issues/detail?id=19))

## TomoTherapy HiArt ##

DICOM Files tested from version 3.1.4, 4.0.2
  * CT Images, RT Structures, RT Plan, RT Dose
  * Differential DVH with 1 cGy bins
    * Converted to a cumulative DVH automatically ([Issue 18](https://code.google.com/p/dicompyler/issues/detail?id=18))
  * Includes Rx Dose with RT Plan
    * Automatically reads the Rx Dose from the RT Plan ([Issue 21](https://code.google.com/p/dicompyler/issues/detail?id=21))
  * Plugins tested:
    * 2D View, DVH, DICOM Tree- work fine ✔
    * Anonymizer does not work if DICOM files were exported to CMS XiO as the DICOM metadata header is missing

## Philips Pinnacle³ ##

DICOM Files tested from version 9.0
  * CT Images, RT Structures, RT Plan, RT Dose
  * No DVH included with RT Dose however DVHs are automatically calculated from the RT Dose directly ([Issue 9](https://code.google.com/p/dicompyler/issues/detail?id=9))
  * Does **NOT** include Rx Dose with RT Plan - **must enter manually**
  * Plugins tested:
    * DICOM Tree, DVH, Anonymizer - work fine ✔
    * 2D View - works fine ✔
      * Works with CT Images and RT Structures
      * RT Dose data is correctly superimposed on the CT Images ([Issue 19](https://code.google.com/p/dicompyler/issues/detail?id=19))

## Prowess ##

DICOM Files tested from version 1.0
  * CT Images, RT Structures, RT Plan, RT Dose
  * Cumulative DVH with 1 cGy bins
  * Includes Rx Dose with RT Plan
  * Plugins tested:
    * DVH, DICOM Tree, Anonymizer - work fine ✔
    * 2D View
      * Works with CT Images and RT Structures
      * RT Dose data is **NOT** shown on the CT Images due to each dose plane being stored in separate RT Dose files ([Issue 14](https://code.google.com/p/dicompyler/issues/detail?id=14))
        * Will be addressed in a future version of dicompyler (unreleased)

## Siemens Syngo RT ##

DICOM RT Ion Files tested from unknown version
  * CT Images, RT Structures, RT Ion Plan, RT Dose
  * Cumulative DVH with **non** 1 cGy bins
  * Plugins tested:
    * DICOM Tree, Anonymizer - work fine ✔
    * 2D View
      * Works with CT Images and RT Structures
      * RT Dose data is **NOT** shown on the CT Images due to incorrect dose scaling
    * DVH works for structures that have not been calculated, otherwise dose scaling is incorrect for DVH

## Varian Eclipse ##

DICOM Files tested from version 8.1, 8.5 and 8.6
  * CT Images, RT Structures, RT Plan, RT Dose
  * Cumulative DVH with 1 cGy bins
  * Includes Rx Dose with RT Plan
  * Plugins tested:
    * 2D View, DVH, DICOM Tree, Anonymizer - all work fine ✔

## Varian BrachyVision ##

DICOM Files tested from version 8.1, 8.5 and 8.6
  * CT Images, RT Structures, RT Plan, RT Dose
  * Cumulative DVH with 1 cGy bins
  * Does **NOT** include Rx Dose with RT Plan - **must enter manually**
  * Plugins tested:
    * 2D View, DVH, DICOM Tree, Anonymizer - all work fine ✔