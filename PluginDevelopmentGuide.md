

# Introduction #

dicompyler is based on a plugin architecture. There are several plugins already included with the main program distribution, however one of the design goals of dicompyler is to allow 3rd party plugins to be developed with ease.

## Types of Plugins ##

Plugins can be one of the following types:
  * A **notebook tab plugin** which gets loaded as part of the main interface. _Example: [2D View](http://wiki.dicompyler.googlecode.com/hg/images/0.3/2dview_mac.png)_
  * A **menu item plugin** which is executed from the Tools menu. _Example: Dose Scaling_ available from the plugin repository.
    * An **export menu plugin** is a just a special version of a menu plugin which exports the loaded DICOM data to a particular format or destination. _Example: [Anonymization](http://wiki.dicompyler.googlecode.com/hg/images/0.2/anonymize_mac.png)_
**New in dicompyler version 0.4.2**:
  * An **import plugin** which is executed from the File -> Import menu. The `'patient.updated.raw_data'` message must be sent by this plugin. _Example: Quick DICOM Import_ which is included with dicompyler.
  * A **2D View plugin** which is executed from the 2D View _Tools_ Toolbar icon. The plugin performs similarly to an **export menu** plugin but is launched from the 2D View Tools item. _Example: [Measurement Plugin](PluginDevelopmentGuide#2D_View_Plugin_Example.md)._


## Plugin Lifecycle ##

Plugins are loaded at initialization of the main program. When dicompyler loads DICOM data, it passes that data to each plugin. At that point, the plugin is free to work with the DICOM data which is represented as dictionary of pydicom `dataset` objects. Additionally, as will be discussed later, the plugin can send and receive information between the main program and also other plugins.

# Implementation #

The following is a general overview of how plugins are implemented in dicompyler.

## Plugin File Structure ##

Most dicompyler plugins consist of a single Python file. The file can be placed either in the _user plugins_ folder:
  * Windows 2000/XP: C:\Documents and Settings\yourusername\Local Settings\Application Data\dicompyler\plugins\
  * Windows Vista/7: C:\Users\yourusername\AppData\Local\dicompyler\plugins\
  * Mac OS X: /Users/yourusername/Library/Application Support/dicompyler/plugins/
  * Linux: ~/.dicompyler/plugins/
or when using the source distribution, in the _baseplugins_ folder.

**Note**: a plugin that is placed in the _user plugins_ folder will override any plugin in the _baseplugins_ folder that has the same file name.

**Note**: As of dicompyler 0.4, if your plugin needs to be separated into multiple files or relies on external resources that you would like to package with your plugin, you may save your plugin within a folder.
Within this folder, you need to to add an file called _init.py_ in your plugin folder that has the following lines:

```
# __init__.py

from modulename import pluginProperties, plugin
```

where `modulename` is your main plugin module name.

## pubsub Data Messaging ##

[pubsub](http://pubsub.sourceforge.net/) was chosen as a way for dicompyler to communicate with plugins, which is the basis of its modular architecture.

There are different messages that the main program sends when certain events happen. This is known as _message publishing_. A plugin can _subscribe_ to one or many of these messages and receive the requested data. Additionally, if a plugin decides to modify data, it can _publish_ a message for a given topic, and any other plugin (or dicompyler itself) that is subscribed to that particular topic will be notified and will load the modified data.

### Types of pubsub Messages ###

The following is a list of messages and data sent with each message that are **sent** by dicompyler as of version 0.2:

| **Message** | **Data** |
|:------------|:---------|
| `'patient.updated.raw_data'` | Python dictionary of raw [pydicom](http://code.google.com/p/pydicom/) objects: `rtss`, `rtplan`, `rtdose`, `images`, and `rxdose` |
| `'patient.updated.parsed_data'` | Python dictionary of parsed DICOM data: `structures`, `plan`, `doses`, `dvhs`, `images`, `rxdose` and demographic data |
| `'structures.checked'` | Python dictionary of structures that have been checked, listed by structure id (integer) |
| `'structures.selected'` | Python dictionary of structure that has been selected with key of `id` |
| `'isodoses.checked'` | Python dictionary of isodoses that have been checked, listed by isodose level (integer) |

**New in dicompyler version 0.4.2** - Additional messages and data sent for 2D View plugins:

| **Message** | **Data** |
|:------------|:---------|
| `'2dview.updated.image'` | Python dictionary of current image number and various properties |
| `'2dview.mousedown'` | Python dictionary mouse coordinates in pixel and mm representation: `x`, `y`, `xmm`, `ymm`|

The dictionary data sent by the `'2dview.updated.image'` message is as follows:

```
{'number':self.imagenum,                # slice number
 'z':self.z,                            # slice location
 'window':self.window,                  # current window value
 'level':self.level,                    # curent level value
 'gc':gc,                               # wx.GraphicsContext
 'scale':self.zoom,                     # current zoom level
 'transx':transx,                       # current x translation
 'transy':transy,                       # current y translation
 'imdata':imdata,                       # image data dictionary
 'patientpixlut':self.structurepixlut}  # pat to pixel coordinate LUT
```

# Examples #

A simple code example is given below to demonstrate the required elements of a **notebook tab** plugin.

## Notebook Tab Plugin Example ##

```
import wx
from wx.lib.pubsub import Publisher as pub

def pluginProperties():
    """Properties of the plugin."""

    props = {}
    props['name'] = 'Test'
    props['description'] = "Display the patient's name"
    props['author'] = 'Plugin Author'
    props['version'] = 0.1
    props['plugin_type'] = 'main'
    props['plugin_version'] = 1
    props['min_dicom'] = ['rtss']
    props['recommended_dicom'] = ['rtss']

    return props

def pluginLoader(parent):
    """Function to load the plugin."""

    panelTest = pluginTest(parent)
    return panelTest

class pluginTest(wx.Panel):
    """Test plugin to demonstrate dicompyler plugin system."""

    def __init__(self, parent):
        wx.Panel.__init__(self, parent, -1)

        # Initialize the panel controls
        self.patnamelabel = wx.StaticText(self, -1, "Patient name:", style=wx.ALIGN_RIGHT)
        self.patname = wx.StaticText(self, -1, "N/A", style=wx.ALIGN_LEFT)
        
        # Set up sizer for control placement
        sizer = wx.BoxSizer(wx.HORIZONTAL)
        sizer.Add(self.patnamelabel, 1, flag=wx.EXPAND|wx.ALL|wx.ALIGN_CENTRE, border=4)
        sizer.Add(self.patname, 1, flag=wx.EXPAND|wx.ALL|wx.ALIGN_CENTRE, border=4)
        self.SetSizer(sizer)
        self.Layout()

        # Set up pubsub
        pub.subscribe(self.OnUpdatePatient, 'patient.updated.raw_data')

    def OnUpdatePatient(self, msg):
        """Update and load the patient data."""
        
        # Get the RT Structure Set DICOM dataset
        rtss = msg.data['rtss']
        self.patname.SetLabel(rtss.PatientsName)
```

### Breakdown of Code Sample ###

There are 2 required functions and 1 required class for a **notebook tab** plugin:
  * Function `pluginProperties` - properties of the plugin (most features will be enabled in future versions of dicompyler)
  * Function `pluginLoader` - entry point of the plugin that will be called by the main program (returns the `wx.panel` class)
  * Class `pluginTest` - actual `wx.panel` that will be displayed (the name of the class is up to the plugin author)

#### Function `pluginProperties` ####

This function returns a Python dictionary that contains the various properties about the plugin. This is used by the main program to determine where and when to load the plugin.

  * `props['name']` - name of the plugin
  * `props['description']` - a brief description of the plugin
  * `props['author']` - author's name
  * `props['version']` - plugin version number
  * `props['plugin_type']` - currently set to `'main'` (other options available in future versions)
  * `props['plugin_version']` - currently `1`
  * `props['min_dicom']` - an array of the minimum DICOM SOP Classes that are required to make the plugin function
    * Current allowed values are `'rtss'`, `'rtplan'`, `'rtdose'` and `'image'`
  * `props['recommended_dicom']` - an array of the recommended DICOM SOP Classes that make the plugin run as best as possible
    * For example, a plugin may minimally require an RT Structure Set: `props['min_dicom'] = ['rtss']`, but works even better if CT data is provided: `props['recommended_dicom'] = ['rtss', 'image']`

#### Function `pluginLoader` ####

This function returns the `wx.Panel` class that will be displayed in the main interface. Not much will differ in this function between plugins, but if further customization is desired, an XRC file can be loaded for static GUI resources.

#### Class `pluginTest` ####

This is the `wx.Panel` class that will be displayed in the main interface. Since dicompyler is built on wxPython, the main portion of the plugin is derived from the wxPython class: `wx.panel`.

The code shows a standard initialization of a `wx.panel` object with two `wx.StaticText` controls. One is a label; the other is a placeholder for the patient's name.

An important statement to note is the subscription of the `'patient.updated.raw_data'` message using the `wx.lib.pubsub` module. This allows the plugin to listen for new patient data, sent by the main program. This particular message contains a dictionary of items that store the various DICOM datasets.

By subscribing to the message, a link is created so that `OnUpdatePatient` is called every time a new message with the topic of `'patient.updated.raw_data'` is sent. In this case, the RT Structure Set is chosen and the patient's name is obtained and displayed.

## Screenshot of Example Notebook Tab Plugin ##

![http://wiki.dicompyler.googlecode.com/hg/images/plugin_test.png](http://wiki.dicompyler.googlecode.com/hg/images/plugin_test.png)

## Menu Item Plugin Example ##

This **menu item** plugin is a functional plugin that will re-sort images based on the axial slice position. The only requirements for this plugin are `image` files. When the menu is clicked, it asks the user for a folder to save the files. It then iterates through the given images, re-sorts them and saves them to disk. When it is done, it shows a dialog box that tells the user how many files were saved and where they were saved to.

**Note:** For a menu plugin, there is no `pluginLoader` function, but instead there is a `pluginMenu` class method. In a future version of dicompyler, both types of plugins will be modified so that they are more similar to each other.

```
import wx
from wx.lib.pubsub import Publisher as pub
import os

def pluginProperties():
    """Properties of the plugin."""

    props = {}
    props['name'] = 'Re-sort Images'
    props['description'] = "Resorts images based on the axial slice position"
    props['author'] = 'Aditya Panchal'
    props['version'] = 0.1
    props['plugin_type'] = 'menu'
    props['plugin_version'] = 1
    props['min_dicom'] = ['images']
    props['recommended_dicom'] = ['images']

    return props

class plugin:

    def __init__(self, parent):

        self.parent = parent

        # Set up pubsub
        pub.subscribe(self.OnUpdatePatient, 'patient.updated.raw_data')

    def OnUpdatePatient(self, msg):
        """Update and load the patient data."""
        
        if msg.data.has_key('images'):
            self.images = msg.data['images']
    
    def pluginMenu(self, evt):
        """Resort images based on the axial slice position."""
        
        slicenums = []
        for image in self.images:
            slicenums.append(image.SliceLocation)
    
        sortedslicenums = sorted(slicenums)
    
        dirdlg = wx.DirDialog(self.parent,
            "Choose or create a folder to save the resorted images...")

        if dirdlg.ShowModal() == wx.ID_OK:
            path = dirdlg.GetPath()
            modality = self.images[0].SOPClassUID.name.partition(' Image Storage')[0]
            for s, slice in enumerate(sortedslicenums):
                for i, image in enumerate(self.images):
                    if (slice == image.SliceLocation):
                        image.InstanceNumber = s+1
                        image.save_as(
                            os.path.join(path, modality + '.' + str(s) + '.dcm'))
            message = str(s+1) + ' images were saved successfully in ' + path + '.'
            dlg = wx.MessageDialog(self.parent, message, 'Resort Images',
                wx.OK | wx.ICON_INFORMATION)
            dlg.ShowModal()
            dlg.Destroy()

        dirdlg.Destroy()
```

## 2D View Plugin Example ##

This **2D View** plugin allows the user to measure distances on the 2D View. The only requirements for this plugin are image files. When the menu item is clicked, The user can click two points on the image and it will then draw a line between the points and display the measurement. If the user clicks the menu item again, they will be able to perform another measurement.

Note: For a 2D View plugin, just like a menu plugin, there is no pluginLoader function, but instead there is a pluginMenu class method. There are also two functions that are called when either the image changes or the mouse button is clicked.

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# distance.py
"""dicompyler plugin that calculates the distance between two points."""
# Copyright (c) 2014 Aditya Panchal

import wx
from wx.lib.pubsub import Publisher as pub
from math import sqrt


def pluginProperties():
    """Properties of the plugin."""

    props = {}
    props['name'] = 'Measure Distance'
    props['description'] = "Measures the distance between two points"
    props['author'] = 'Aditya Panchal'
    props['version'] = '0.4.2'
    props['plugin_type'] = '2dview'
    props['plugin_version'] = 1
    props['min_dicom'] = ['images']
    props['recommended_dicom'] = ['images']

    return props


class plugin:

    def __init__(self, parent):

        self.parent = parent

        # Set up pubsub
        pub.subscribe(self.OnUpdateImage, '2dview.updated.image')
        pub.subscribe(self.OnMouseDown, '2dview.mousedown')

        # Plugin is not ready to measure until the menu has been launched
        self.start_measuring = False

    def pluginMenu(self, evt):
        """Start the measure distance plugin."""

        # Set up variables
        self.point_one = None
        self.point_two = None
        self.start_measuring = True
        self.z = 0

        # Refresh the 2D display to get the latest image data
        self.parent.Refresh()

    def OnUpdateImage(self, msg):
        """Update and load the image data."""

        if self.start_measuring:
            # Get the image data when the 2D view is updated
            self.imagedata = msg.data
            self.gc = self.imagedata['gc']
            self.gc.Scale(self.imagedata['scale'], self.imagedata['scale'])
            self.gc.Translate(self.imagedata['transx'],
                              self.imagedata['transy'])
            self.DrawMeasurement()

    def OnMouseDown(self, msg):
        """Get the cursor position when the left mouse button is clicked."""

        # Make sure that we are measuring
        # This only occurs after the plugin has been launched via the menu
        if self.start_measuring:

            # Get the mouse cursor position point
            point = msg.data

            if (self.point_one is None):
                self.point_one = point
                # Record the z plane of the first point
                self.z = self.imagedata['number'] - 1
            elif (self.point_two is None):
                # Make sure that the second point is on the same z plane
                if (self.z == self.imagedata['number'] - 1):
                    self.point_two = point
                    # Measure the distance between the two points
                    self.MeasureDistance()
                # Otherwise re-obtain first point since this is a new z plane
                else:
                    # Record the z plane of the first point
                    self.z = self.imagedata['number'] - 1
                    self.point_one = point

    def MeasureDistance(self):
        """Measure the distance between the two points."""

        # Get the differences between the two points
        px = self.point_one['xmm'] - self.point_two['xmm']
        py = self.point_one['ymm'] - self.point_two['ymm']

        # Calculate the distance
        # Distance is reported in mm so convert to cm
        self.dist_cm = sqrt((px) ** 2 + (py) ** 2) * 0.1

        # Refresh the 2D display to draw the measured distance
        self.parent.Refresh()

    def DrawMeasurement(self):
        """Draws the measurement line."""

        # Make sure that the second point has been clicked
        if not (self.point_two is None):

            # If the slice number doesn't match the z plane
            # don't draw the measurement line
            if not (self.z == self.imagedata['number'] - 1):
                return

            # Set the color of the line
            c = wx.Colour(255, 0, 0)
            self.gc.SetBrush(wx.Brush(c, style=wx.TRANSPARENT))
            self.gc.SetPen(wx.Pen(c, style=wx.SOLID))

            # Create the drawing path for the measurement line
            path = self.gc.CreatePath()
            # Draw the measurement line
            path.MoveToPoint((self.point_one['x'], self.point_one['y']))
            path.AddLineToPoint((self.point_two['x'], self.point_two['y']))
            # Close the subpath
            path.CloseSubpath()
            # Draw the final path
            self.gc.DrawPath(path)
            # Draw the measurement distance text
            self.gc.DrawText('%.2f' % self.dist_cm + ' cm',
                             self.point_two['x'] + 2, self.point_two['y'] + 2)
```

# Additional Notes #

## Export Menu Plugins ##

Export menu plugins function identically except for the fact that they are located under the _Export_ menu. Consider making a plugin an export plugin if it saves data to disk in a specific format or to a network destination.

The only changes required for an export menu plugin is to add the following lines to your `pluginProperties` dictionary:

```
props['plugin_type'] = 'export'
props['menuname'] = "as XML"
```

where:
  * `props['plugin_type']` is specified as `'export'` so dicompyler understands to place it under the Export menu
  * `props[menuname]` is the title of the menu so that it flows smoothly in English with the prefix `'Export'`, i.e. if your plugin exported XML data as above, the menuname should be `"as XML"`

## Import Menu Plugins ##

Import menu plugins allow data to be imported from various sources. As long as the data is converted to a series of raw pydicom objects, it can be used within dicompyler. A dictionary of objects following the `'patient.updated.raw_data'` message standard must be sent via pubsub. dicompyler will automatically receive this message and load the appropriate plugins.

The only other changes required for an export menu plugin is to add the following lines to your `pluginProperties` dictionary:

```
props['plugin_type'] = 'import'
props['menuname'] = "DVH Text File..."
```

where:
  * `props['plugin_type']` is specified as `'import'` so dicompyler understands to place it under the Import menu
  * `props[menuname]` is the title of the menu so that it flows smoothly in English with the prefix `'Import'`, i.e. if your plugin imported DVH Text files as above, the menuname should be `"DVH Text File..."`

## dicompyler Plugin Repository ##

If you feel that your plugin is worthy of sharing, consider submitting it to the [dicompyler Plugin Repository](http://code.google.com/p/dicompyler-plugins/). You can have your plugin hosted for free, include a wiki for your plugin, and it can be listed within the dicompyler plugin browser for all users to see.

For further information, take a look at the [Repository Plugin Author Guide](http://code.google.com/p/dicompyler-plugins/wiki/RepositoryPluginAuthorGuide).