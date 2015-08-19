# Build requirements and dependencies #

There are several items that need to be installed before you can use dicompyler. Since this project is cross-platform, it is assumed you should be able to run Python on Windows / Mac / Linux. Most dependencies can be installed via [easy\_install](http://pypi.python.org/pypi/setuptools) or [pip](http://pypi.python.org/pypi/pip).

**New:** As of version 0.4.1, dicompyler is now a package and can be found on [PyPI](http://pypi.python.org/pypi/dicompyler/). To get the latest released version, just install via: `easy_install dicompyler`
or
`pip install dicompyler`.

If you are interest in the source code for dicompyler, it is available [here](http://code.google.com/p/dicompyler/source/checkout).

# Details #

**Main requirements:**
  * [Python](http://www.python.org) 2.5 or higher; untested on Python 3.0
    * Does NOT work on the system Python 2.6 on Mac OS X Snow Leopard due to wxPython not being compatible
      * Use the command python2.5 to run
      * Install packages with easy\_install-2.5
  * [wxPython](http://www.wxpython.org) 2.8.8.1 to 2.8.10.1 (2.9 is not supported yet)
  * [Python Imaging Library](http://www.pythonware.com/products/pil/) (PIL) 1.1.7 or higher or any version of [Pillow](https://pillow.readthedocs.org/en/latest/)
  * [simplejson](http://github.com/simplejson/simplejson) (**ONLY for Python 2.5** - Python 2.6+ includes JSON support)

**Scientific requirements:**
  * [pydicom](http://code.google.com/p/pydicom/) 0.9.5 or 0.9.6
    * **IMPORTANT**: Currently pydicom 0.9.7 does not work due to the way Decimal data is handled - [more information](https://groups.google.com/d/topic/dicompyler/QxqJPESN8WA/discussion)
  * [NumPy](http://www.scipy.org/Download) 1.2.1 or higher
  * [matplotlib](http://matplotlib.sourceforge.net) 0.99 to 1.1 (not 1.2 or higher)

# Getting the Source #

The dicompyler code is managed with [mercurial](http://mercurial.selenic.com/) (a.k.a. hg). In order to download the dicompyler code, you will need to have hg installed. Once hg is installed, you can clone (download) the code repository with the following command:

```
hg clone https://dicompyler.googlecode.com/hg/ dicompyler
```

Further instructions and the browseable code are found [here](http://code.google.com/p/dicompyler/source/checkout).

# Building #

As Python is an interpreted language, the source is simply run rather than built or compiled. Any build steps necessary for libraries should be explained in included documentation.

# Running #

The most basic way to run dicompyler is to call dicompyler\_app.py while in the dicompyler directory.

```
user@computer:~/dicompyler$ ./dicompyler_app.py
```

After dicompyler has started, open a patient by selecting the Open Patient button or using the File menu.

# Updating to the Latest Source #

To get the latest dicompyler source code, use the following mercurial command while in the dicompyler directory, which downloads the code:

```
hg pull
```

then use the following command which actually applies the code to your local copy:

```
hg update
```

If you have made any changes, there will be conflicts. This is outside of the scope of this guide, but you will have to resolve the changes before performing another operation with mercurial. Please see this [guide](http://hginit.com/01.html) for further information on mercurial.

# Developing, etc. #

Please see the [Plugin Development Guide](http://code.google.com/p/dicompyler/wiki/PluginDevelopmentGuide) and the [Helping](Helping.md) page.