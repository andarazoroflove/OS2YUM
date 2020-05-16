A roundabout way to get yum working on OS/2 or eComStation.

Recently had an older ThinkPad R60e left on my desk. Its 
vintage-ish, being 15 years old and it still says IBM on 
it rather than Lenovo, so I thought time to try OS/2 on 
hardware rather than in a VM. 

Using the OS/2 Warp 4.52 .iso from WinWorld PC, everything
installed easily and it even found a generic Wifi driver
which worked out of the box. Now, how do I install packages?
The prevailing information out there is to use Arca Noae 
Package Manager, and it does install easily. Unfortunately
it crashes/freezes immediately upon trying to reboot after
its first update and thus its useless to me. 

Next up, there is an older method from netlabs which uses a 
Yum Boostrapper to install yum in to the filesystem. This one 
installs fine but the packages on the repository are newer
than the bootstrapper and they fail a circular redunancy when
running "yum install yum" on first load.

I dumped the contents of "yum install yum" in to the file
dependencies.txt included here, then downloaded each file from
the netlabs repository in the correct architecture and version.
Those files are included in the rpms directory.

To get yum to work for OS/2 Warp or Ecommstation:

1: Clone this repository on to a USB stick or CD and mount it on OS/2

2: Use WarpIn to install the rpm-yum-bootstrap-1_5-p4.wpi file and 
it will create a Yum Bootstrap folder on the desktop. REBOOT the system.

3: Open the Yum Boostrap Console from inside the folder and let it do
its initial set-up. Once it finishes, DO NOT DO YUM INSTALL YUM! Instead,
change to the drive you mounted the rpms on and do the following:

D:\OS2YUM\>rpm -i --nodeps rpms\*.rpm

This will install all the rpms needed without complaining about the 
circular redundencies. This is ONLY THE YUM/RPM install, so once done
you can install other items from the netlabs repository. When it asks
for a UNIXROOT= just put in C:

4:Once all the rpms are installed, edit the C:\Config.sys file with
the OS/2 Text Editor and add this to the section called LIBPATH=

;C:\USR\LIB;C:\EMX\LIB;

You can just append it to the end of whatever is on there now, ; are the
separator.

5: Add this to the section called PATH=

;C:\BIN;C:\USR\BIN;C:\EMX\BIN;

6: Add this line at the bottom of the file:

UNIXROOT=C:

7: Close the Yum boostrapper and reboot. THIS IS IMPORTANT. 

Now, when the system comes back up, you can fire up the OS/2 Command
Shell and do "yum update" and it will connect to the netlabs-rel 
repository and update. You can then do any installs you like with
"yum install $package(s)." Last but not least, you can also uninstall
the Yum Bootstrapper using WarpIn. 

The other files in the repository will enable you to install Firefox
once your yum install is working. Here's how you can do that:

1: From your OS/2 Command Prompt, install the firefox dependencies:

C:\>yum install libstdc++ nspr nss libicu pixman cairo pango fontconfig
freetype libkai libvpx libjpeg-turbo libpng zlib bzip2 hunspell libcx

2: Run the WarpIn installer for either 38 or 45 from the browser folder
and install the browser you'd like. 

If you'd prefer 17, you can unzip it and copy the items from the mozsupport
folder to your C: and link them in to your config.sys. 
