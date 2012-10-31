Preseeding d-i
==============

Preseeding provides a way to set answers to questions asked during the
installation process, without having to manually enter the answers while
the installation is running. This makes it possible to fully automate most
types of installation and even offers some features not available during 
normal installations.

Most of the questions asked by DebianInstaller_ can be preseeded by setting the
answers in the `debconf`_ database. The Installation Guide includes an `extensive appendix`_ dedicated to preseeding. For concrete preseed files look below. 
Feel free to add any information that is not covered in the manual to the 
notes below.

Preseeding methods
------------------

As mentioned in the `official installation guide`_, there are several ways to
feed the preseed file to the installer.

Adding the preseed file to the installer's initrd.gz
----------------------------------------------------

Installation can be fully automated by adding a preseed file to the installer
ISO's initrd.gz. This method is described in detail in `this wiki article 
<http://wiki.debian.org/DebianInstaller/Preseed/EditIso>`_. The downside of 
this method is that net installer has to be generated whenever a preseed file 
is modified.

Autoloading the preseeding file from a webserver via DHCP
---------------------------------------------------------

If you have control over the DHCP server on your network, this method allows
fully automated installations; as demonstrated and documented at Hands-Off_

Loading the preseeding file from a webserver
--------------------------------------------

Most install methods you can interrupt early on and add a URL to a preseed 
file, for an almost fully automated installations.  Here exemplified with the
graphical installer:

* When the graphical installer menu appears, press ESC
* (Type ``help`` if you want view generic help)
* Type ``auto url=http://webserver/path/preseed.cfg``, replacing the URL with 
the address to your preseed configuration file

The ``auto`` command launches the installation in the automated mode, where 
the configuration of hostname, locale and keymap are postponed so that they 
can be answered from the preseed file loaded from the network. You could use 
``install url=...`` but you'd have to answer these questions manually, 
regardless of what you have in the preseed config.

Default preseed files
---------------------

When creating a preseed file, you should start from a known good, default 
preseed file:

* `Preseed file example`_ for `Debian Stable`_.

If you use a preseed file for an older, newer or otherwise different OS, you
will most likely be prompted for answers at some point, even if you thought
you automated everything.

Examples
--------

Post here any links you have to example preseed files. Note that using any of
these files directly is not wise, as a malicious person could probably come up
with values for a preseed file that makes d-i misbehave. Also, the files are
downloaded over http, so are vulnerable to man-in-the-middle spoof attacks.
The best way to use any preseed file is to copy it to your own local web server
or media, and look it over before using it.

- Christian Perrier's page documenting automated d-i installs in vmware, using
netboot.  http://people.debian.org/~bubulle/d-i/vmware-fai.html

- Holger Levsen's d-i examples showing a way to preserve partitions and
ssh-host-keys: http://layer-acht.org/d-i/

- Enrico Zinis conditional partitioning hints:
http://www.enricozini.org/2008/tips/d-i-conditional-partitioning/

- Using network-console and preseeding is described on two pages,
`DebianInstaller/NetworkConsole <http://wiki.debian.org/DebianInstaller/NetworkConsole>`_
and `DebianInstaller/Remote <http://wiki.debian.org/DebianInstaller/Remote>`_.

- Phil Hands' d-i setup, that allows minimal (i.e. no exim) installs, works
from CD, PXE & USB (read the HOWTO's), and allows custom configs to be
specified at the boot: prompt http://hands.com/d-i/

- `DebianInstaller/AsSshClient <http://wiki.debian.org/DebianInstaller/AsSshClient>`_
for using d-i as a ssh terminal

- `Instalinux <http://www.instalinux.com/>`_ lets you answer a few questions on the web and generate an ISO
image that can be used to install Debian noninteractively, or a preseed file
that you can use with other install methods.

- Christian Perrier documented a D-I demo setup in `Babel Box <http://wiki.debian.org/DebianInstaller/BabelBox>`_

- Debian Administration has an article on using preseeding:
http://www.debian-administration.org/articles/394

- Filip Van Raemdonck documented modifying an iso to include the preseed file
in `DebianInstaller/Preseed/EditIso <http://wiki.debian.org/DebianInstaller/Preseed/EditIso>`_.

- GüSengüh a very simple preseed.cfg for fully automatic installation of 
workstations using dphys-config:  http://debian.ethz.ch/d-i/p (Used for i386,
amd64 installs on a wide variety of hardware configurations, with a wide
variety of use of the computers. Including large repositories of special
software)

- Matt Taggart's notes and configuration, including using serial console and
postfix. http://lackof.org/taggart/hacking/d-i_preseed/

- Step by Step guide on how to integrate non-free firmware and preseed.cfg
`Remaster Netinstaller image`_.

Notes
-------

- Do not work off a ``debconf-get-selections`` (``--installer``) generated
``preseed.cfg`` but get the values from it and modify the example preseed file
with them.

- Be aware there is only one space in preseed files between subkey and value on
``owner key/subkey value`` lines.

- Do not reboot in the ``base-config/late_command`` command, the installation
process will start again at the start of the 2nd stage.

- Preseeding has changed significantly in etch, preseed files for sarge will
need to be updated or re-done. The largest change is the removal of
base-config, which means that ``base-config/late_command`` and 
``base-config/early_command`` are no longer available.

- To install additional packages in etch, you can
``preseed preseed/early_command`` to run ``apt-install package``.

- Look in ``debconf-devel(7)`` in the ``debconf-doc`` package for more docs
about d-i and debian-installer preseed questions.

- If your preseed value is being ignored and whilst using DEBCONF_DEBUG=5 to
watch the debconf output you see ``FSET blah false`` it just means that a
piece of code really wants that question to be seen, and such questions are
not normally preseedable - the only way to avoid them is to avoid the
situation that gives rise to that question being asked.

.. _official installation guide: http://www.debian.org/releases/stable/i386/apb.html
.. _extensive appendix: http://www.debian.org/releases/stable/i386/apb.html
.. _debconf: http://wiki.debian.org/debconf
.. _DebianInstaller: http://wiki.debian.org/DebianInstaller
.. _Hands-Off: http://hands.com/d-i/
.. _Preseed file example: http://www.debian.org/releases/stable/example-preseed.txt
.. _Debian Stable: http://wiki.debian.org/DebianStable
.. _Remaster Netinstaller image: http://www.n0r1sk.com/index.php/Debian_Remaster_Netinstaller_-_Integrate_Firmware_bnx2x_and_Preseed
