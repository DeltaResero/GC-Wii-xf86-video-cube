****************************************
**_XFree86 driver for Gamecube/Wii_**
****************************************

This repository was created here to help in maintaining the long abandoned XFree86 driver by Nuvalo for GameCube/Wii consoles.  It was originally imported from CVS from the following location:  
"http://gc-linux.cvs.sourceforge.net/viewvc/gc-linux/xf86-video-cube".  

**Index**  
**1)** - Introduction   
**2)** - How to compile  
**3)** - How to install  
**4)** - Configure xorg.conf  
**5)** - Credits  

<br>

****************************************
**1) - Introduction**
****************************************

This driver implements only the basics for the xserver to work with correct colours. It doesn't implements extensions like GLX, or Xvideo (maybe in the future...). This work is based on the Xf86 glide driver, and has parts taken from SDL-gclinux lib, fbdev xf86 driver and glide xf86 driver compatible with GC/Wii-Linux.  

This driver supports:  
640x480-16bpp screen configuration  

This driver lacks:  
- Other screen configurations...  
- Xvideo, XGL, and any other X extension...  
- Problems in PAL consoles: As pal draws in 640x576, you will see a green line in the bottom of the screen.  To solve, put the video mode in 480i/p/60Hz.  

<br>

****************************************
**2) - How to compile:**
****************************************

You will need a gcc compiler, and the xserver-xorg development libraries and headers installed on your system.  On a Debian based system, these packages can be installed using the following command:  

    sudo apt-get install autoconf automake gcc libtool libxrandr-dev make module-init-tools m4 pkg-config xserver-xorg-dev xutils-dev x11proto-fonts-dev x11proto-randr-dev x11proto-video-dev

- *This list doesn't contain dependencies of dependents, package managers should handle this.*  
- *Some dependencies may have been missed.  If a direct dependency is found, append it the list.*
<br>
<br>
To build the driver form source, uncompress the tarball with the sources (if not already uncompressed), and type the following into a terminal:

        "./autogen.sh && ./configure && make"

<br>

****************************************
**3) - How to install:**
****************************************

If you downloaded the precompiled binaries, you can install the driver manually.  You need an already installed xorg-server environment. Copy the contents inside the "bin" directory (cube_drv.so, cube_drv.la) to the xserver drivers directory ("/usr/local/lib/xorg/modules/drivers","/usr/lib/xorg/modules/drivers" or "/usr/X11R7/lib/xorg/modules/drivers"), and
configure the xserver to use this driver.  For Debian based operating systems, if the driver was built on the Wii requiring the driver, the following
can be done to copy the module into the correct location:  

	"sudo make install"
	"sudo cp /usr/local/lib/xorg/modules/drivers/cube_drv.* /usr/lib/xorg/modules/drivers"

May also need to run the following to switch drivers  

	"sudo dpkg-reconfigure xserver-xorg"

(Xorg will need restarted before the driver can used following the installation)  

<br>

****************************************
**4) - How to configure "/etc/X11/xorg.conf":**
****************************************

- Edit the file "/etc/X11/xorg.conf" and add a new video section:  

        Section "Device"
            Identifier  "WII/Gc Card"
            Driver      "cube"
        EndSection

- Edit the module section, and leave it as this:

        Section "Module"
            Load    "dbe"
            Load    "ddc"
        SubSection  "extmod"
            # Option    "omit xfree86-dga"
            # Option    "omit XFree86-VidModeExtension"
        EndSubSection
            Load        "type1"
            Load        "freetype"
            Load    "dri"
        EndSection

Edit the screen section, and replace the next options:  

        Section "Screen"
                ....
                Device        "WII/Gc Card"
                ....
                DefaultDepth    16
                Subsection "Display"
                    Modes       "640x480"
                EndSubsection
        EndSection

<br>

***************
**5) - Credits:**
***************

**This driver was made possible thanks to work of many other people:**  
- Marcos Novalbos (Nuvalo), for his creation of this driver (based on Xfree86 glide)   
- Albert Herranz, for his SDL Gamecube driver for Wii Linux.  
- Henrik Harmsen, for his glide Xfree86 driver which gave the basic structure.  
- Alan Hourihane and Michel Dänzer, for their framebuffer xfree86 driver.  
- All the GC-Linux team, for their efforts in porting Linux to the Gamecube/Wii consoles and any other contributors that may have been missed.  
