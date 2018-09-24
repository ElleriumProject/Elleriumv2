Debian
======

This directory contains files used to package elleriumd/ellerium-qt
for Debian-based Linux systems. If you compile elleriumd/ellerium-qt yourself, there are some useful files here.

## ellerium: URI support ##

ellerium-qt.desktop (Gnome / Open Desktop)

To install:

	sudo desktop-file-install ellerium-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your ellerium-qt binary to `/usr/bin`
and the `../../share/pixmaps/ellerium128.png` to `/usr/share/pixmaps`

ellerium-qt.protocol (KDE)