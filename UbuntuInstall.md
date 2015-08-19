# Introduction #

dicompyler user [Andrew Miller](http://www.uow.edu.au/~amiller/) has contributed this guide for beginners to install dicomyler on Ubuntu (10.10 in this case). Thanks Andrew!


# Installing dicompyler on Ubuntu for newbies #

The dicompyler installation requires:

**the use of the terminal (a.k.a. Command Line Interface, 'command line' to most users)** a working internet connection

My PC is called “andrew@jesse-GA-N650SLI-DS4L”. When you see this phrase, you can substitute the name of your own PC. This is the first line you will see when you open a terminal. So open a terminal and you should see something like this:
```
andrew@jesseGAN650SLIDS4L:
```
Now type/copy & paste the following text after the colon (its all one line):
```
sudo apt-get install mercurial python2.7 python2.7-minimal python-wxtools 
python-imaging python-numpy python-matplotlib python-elixir python-sqlalchemy 
python-setuptools
```
Press ENTER (I won't tell you to do this again, but after each line you need to press
enter), and the system will respond with:
```
[sudo] password for andrew:
```
Obviously you will see your own name there, so type in your superuser/admin password (its the same as the one you use to log in!). Your packages will start to download and eventually will install, and you will return to the prompt. You have just installed the dependencies [listed](BuildRequirements.md) on the dicomplyer website – Python, wxPython, Python Imaging Library, NumPy, matlibplot, SQLAlchemy and Elixir. You have not installed pydicom yet though. You can now issue the command to get python package.
```
sudo easy_install pydicom
```
You will see the following text appear in the terminal:
```
install_dir /usr/local/lib/python2.6/distpackages/
Searching for pydicom
Reading http://pypi.python.org/simple/pydicom/
Reading http://pydicom.googlecode.com
Reading http://code.google.com/p/pydicom/downloads
Best match: pydicom 0.9.5
Downloading http://pydicom.googlecode.com/files/pydicom0.9.5.
zip
Processing pydicom0.9.5.
zip
Running pydicom0.9.5/
setup.py q
bdist_egg distdir
/tmp/easy_installRbRdIL/
pydicom0.9.5/
eggdisttmpkNEFA6
Adding pydicom 0.9.5 to easyinstall.
pth file
Installed /usr/local/lib/python2.6/distpackages/
pydicom0.9.5py2.6.
egg
Processing dependencies for pydicom
Finished processing dependencies for pydicom
```
Now you need to pull the python code from the Googlecode repository. On the command line enter:
```
hg clone https://dicompyler.googlecode.com/hg/ dicompyler
```
You will see the following text appear in the terminal:
```
requesting all changes
adding changesets
adding manifests
adding file changes
added 90 changesets with 237 changes to 53 files
updating to branch default
51 files updated, 0 files merged, 0 files removed, 0 files
unresolved
```
If you look in your home directory, you will now see a new directory /dicompyler in your /home/my\_name directory. This is where the dicompyler files are found.

To obtain some test DICOM data for your new installation, open [this link](http://code.google.com/p/dicompyler/downloads/detail?name=testdata.zip&can=2&q=) in your browser and download the file testdata.zip. For convenience, save the ZIP file in the folder /home/my\_name/dicompyler/testdata/. You now need to uncompress the ZIP file.
Enter the directory by entering the following into the command line:
```
cd dicompyler/testdata
unzip testdata.zip
```
You will see the following text appear in the terminal:
```
Archive: testdata.zip
...
inflating: rtdose.dcm
inflating: rtplan.dcm
inflating: rtss.dcm
```
Now enter the dicompyler directory and run the code:
```
cd ..
./main.py
```
You should now be greeted by the dicompyler screen (if not, try `python main.py`). You can then select the patient. If you have a DICOM-RT file from your work, park it in testdata also.

Andrew Miller, January 28, 2011

---

**An update From Sébastien in [Issue 85](https://code.google.com/p/dicompyler/issues/detail?id=85) regarding installing specific versions of packages, i.e. pydicom:**

The currently installed version of pydicom can be obtained through 2 equivalent commands:

`# pip search pydicom`

`# python -c "import dicom; print dicom.__version__"`

To force pydicom 0.9.8 to be installed, type:

`# sudo pip install pydicom==0.9.8`