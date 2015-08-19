# Frequently Asked Questions #

  * ### Where do I find documentation and information about dicompyler? ###
Most of the information and documentation is found in the [Wiki](http://code.google.com/p/dicompyler/w/list) pages. Pages cover [News](News.md), [Release notes](ReleaseNotes.md), [Build requirements](BuildRequirements.md), [Helping](Helping.md), [License](License.md), and [more](http://code.google.com/p/dicompyler/w/list).

  * ### Where do I get help with dicompyler or ask questions? ###
If the answer to your question is not found in the [Wiki](http://code.google.com/p/dicompyler/w/list) pages, please post your question to the [dicompyler discussion group](http://groups.google.com/group/dicompyler). If your problem is the result of a bug in dicompyler, we encourage you to submit an [issue](http://code.google.com/p/dicompyler/issues/list) report.

If your question seems to be beyond the scope of dicomypler, you might consider posting to the [Python in Medical Physics](http://groups.google.com/group/python-medphys) group.

  * ### What can I do with dicompyler? ###
dicompyler is fully open source and uses a plugin architecture. That means you are allowed to do anything you want, within the limits of the BSD-style [License](License.md). The open source licensing and plugin architecture means that dicompyler is highly extensible. If you are interested in writing plugins or developing the base code, see the [Plugin development guide ](PluginDevelopmentGuide.md) and the [Helping](Helping.md) wiki pages.

General features of dicompyler include loading, displaying, and manipulating DICOM and DICOM-RT data files. Current and future features of dicompyler are listed in the [Release notes](ReleaseNotes.md) and the [Roadmap](Roadmap.md).

Features as of version 0.3 (October 2010):
  1. Import DICOM and DICOM-RT files
  1. Windows, Linux, Mac OS X support
  1. Display 2D images
  1. Zoom and pan, scroll image stack, adjust window/level
  1. Display existing contoured structures and isodose contours
  1. Display cumulative DVHs
  1. DICOM anonymization


  * ### Will my data work with dicompyler? ###
The DICOM standard is implemented inconsistently across vendors and software producers. We have tried to make dicompyler as robust as possible with regards to the differences and quirks, but it is an ongoing process. Please see the dicompyler [DICOM compatibility guide](DICOMCompatibilityGuide.md) for the latest information that we have compiled. Please post to the dicompyler [discussion group](http://groups.google.com/group/dicompyler) if you have questions or information for updating the guide.



  * ### Can you provide me with some test DICOM data? ###
A set of test DICOM-RT data (with CT images) can be found in the [Downloads](http://code.google.com/p/dicompyler/downloads/list) section. It is likely listed as [testdata.zip](http://code.google.com/p/dicompyler/downloads/detail?name=testdata.zip&can=2&q=).