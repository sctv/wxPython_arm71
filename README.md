# wxPython_arm71
compiled wxPython-4.0 for arm71/rpi2 on raspian
================================================
adapted from [https://wiki.wxpython.org/BuildWxPythonOnRaspberryPi?action=raw] this is based on instructions for building vxPython for python3, but adjusted for python 2.7 Main change is that we use virtenv instead of venv

#language en = Build wxPython On Raspberry Pi = <> '''Instructions by''' [[https://wiki.wxpython.org/JamesKey | James Key]] Modified by github.com/sctv

'''These are the build and install instructions. '''

This does not seem to work with GTK3 at the moment. So we will stick with GTK2.

'''READ THEM ALL FIRST!'''

Especially the note at the end.

== Build and Install: ==
Okay so you want or need wxPython 4.0.x on your Raspberry Pi or you like to watch lots of messages scroll by on long builds. Either way this is how to do it.

First things first MAKE A VIRTUAL ENVIORNMENT for the build and install. Okay this isn't strictly needed but if you don't know about venv or virtual environments, they do keep the environment cleaner for each project. The only issue is that you need to run the pip install for each virtual environment you want to use it in. It's your choice.

These instructions are based on the Github Phoenix README:

https://github.com/wxWidgets/Phoenix/blob/master/README.rst

Okay now to make the virtual environment. I made mine right off the /home/pi directory but you can put it anywhere. Some like to put all there virtual environments (virtenv) in specific locations.

First install vertenv

$ pip install virtualenv
Lets make one (wx27 is the name of the virtenv we are creating, change it to suit):

    $ cd ~

    $ virtualenv -p python wx27

    $ source wx27/bin/activate
The last line activates the environment. Your prompt should change to show that you are in an active virtualenv. To deactivate type deactivate. If you log out, close the terminal, or restart the Pi, or open a new terminal, you need to run the activate command again. If not you won't be in the virtual environment.

First thing you need is wxPython. Go to:

https://pypi.python.org/pypi/wxPython/ or https://pypi.python.org/pypi/wxPython/4.0.1

The first link will give you a choice of releases, as before I tested 4.0.0b2 and it works, but you might get here later and a newer release is available. PLEASE use the tarball from these links, I too wanted the latest, greatest version. Don't. If you download it from github you need to do a massive amount of work. '''''Let the marvelous people who developed this package do that, then thank them.'''''

Down at the bottom of the choices you'll see ''“wxPython-4.0.1.tar.gz … Source”''.

Download wxPython-4.0.1.tar.gz

Use your browser if you can. The the link currently is the very human unfriendly:

https://pypi.python.org/packages/a4/4a/d35b19f8c21b414ece2b3dc02dcf8cb0d45d6fe5aacf56f7ca0b7c66ac58/wxPython-4.0.1.tar.gz#md5=a56ece3363da3949dd0f30c45cc21631

'''''Ick.''''' Okay that's not exactly a professional comment but sometime you just have to call it what it is.

    $ cd ~

    $ mv ~/Downloads/wxPython-4.0.1.tar.gz .

    $ tar xf wxPython-4.0.1.tar.gz
    ```
Okay here we go. Let's make sure we have the dependencies.
$ sudo apt-get update

$ sudo apt-get install dpkg-dev build-essential libjpeg-dev libtiff-dev libsdl1.2-dev libgstreamer-plugins-base0.10-dev libnotify-dev freeglut3 freeglut3-dev libwebkitgtk-dev
pre-installed Python:
$ sudo apt-get install python2.7-dev
Next step, make sure you are in the environment you want to install wxPython into.

    $ cd wxPython-4.0.1
    $ pip install -r requirements.txt
Okay the next part is the big one. This will take a while, anywhere from a few hours, to 18+ hours on a Pi zero.

    $ python build.py build bdist_wheel --jobs=1 --gtk2
TODO-EDIT THE BELLOW TEXT TO REFLECT 2.7 Now to install it in Python as a package:

    $ cd ~/wxPython-4.0.1/dist

    $ pip3 install wxPython-4.0.1-cp36-cp36m-linux_armv7l.whl
deactivate vertenv

[server]$ deactivate
Now let’s test it:
$ cd ~/wxPython-4.0.1/demo

$ python demo.py
can also delete the environment directory 
rm -rf ~/wx27

== Notes: ==
The build.py script has many options, but I will warn you unless you
know what your doing, don't play with them. I had built the package for
ver 3.4 then decided to try 3.6. So I ran the cleanall option to clean
out the last builds binaries. It did and it cleaned all the SIP and
Doxygen bits out too. That defeated the whole point of me getting the
pypi tarball. To clean up, delete the directory and extract it again.

Beware you may need an SD card larger than 16MBs

'''**Other things to note**'''
There were a few times that my pi seemed to
freeze. Leave it alone for awhile. Give it 15-20 minutes. If it doesn't
recover unplug it, enable the virtenv and restart by using your last
command. Usually the 'pip install'. Above all don't panic.

which i adapted from [this page](https://wiki.wxpython.org/BuildWxPythonOnRaspberryPi) which built it for python 3 [works]  
