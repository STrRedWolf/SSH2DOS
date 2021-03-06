This repo contains a fork of SSH2DOS, originally on SourceForge.
Original readme file is in docs/readme
Modifications by Kelly "STrRedWolf" Price and will be documented as appropriately.
Licence remains GPL under the docs/copying file.

-----

# SSH2DOS v0.2.1 Copyright (c) 2000-2006 Nagy Daniel
# Release date: 04-23-2006


## COPYRIGHT

SSH2DOS
Copyright (c) 2000-2006 Nagy Daniel
This program is distributed under the terms of the GNU General
Public License. Please read the copying file for details.

Portions:

WATT-32 library (which is based on the WATTCP library)
Copyright (c) 1990, 1991, 1992, 1993 Erick Engelke
Portions Copyright (c) 1993  Quentin Smart
Portions Copyright (c) 1991  University of Waterloo
Portions Copyright (c) 1990  National Center for Supercomputer Applications
Portions Copyright (c) 1990  Clarkson University
Portions Copyright (c) 1983, 1986, Imagen Corporation
http://www.wattcp.com
http://www.bgnett.no/~giva

ZLIB library
Copyright (C) 1995-1998 Jean-loup Gailly and Mark Adler
http://www.gzip.org

PuTTY
Copyright (c) 1997-2002 Simon Tatham
PuTTY is distributed under the terms of the MIT licence.
http://www.chiark.greenend.org.uk/~sgtatham/putty/

CVT100
Copyright (c) 1988 Jerry Joplin (CVT100)
Portions copyright (c) 1981, 1988 Trustees of Columbia University in the City of New York
Permission is granted to any individual or institution
to use, copy, or redistribute this program and
documentation as long as it is not sold for profit and
as long as the Columbia copyright notice is retained.
http://www.simtel.net/pub/simtelnet/msdos/commprog/cvt100.zip


## INTRODUCTION 

SSH2DOS (SSH2D386) is an SSH client which provides a telnet-like
interactive login shell to remote hosts. It can be used to run
commands on remote hosts as well.

SFTPDOS (SFTP386) and SCPDOS (SCP2D386) are secure file transfer
utilities capable of transferring files from remote to local or
from local to remote machines.

TELNET (TEL386) is the good old telnet utility.

These programs can run on low-end machines (8086+) when
compiled as real-mode applications (OpenWatcom large model),
so it's an ideal solution to connect from el-cheapo machines
or DOS compatible PDAs.
The 386 version (OpenWatcom flat model) is much faster,
	but it requires at least a 386 machine.

Supported cipher: AES
Implemented SSH protocol version: 2.0
Supported authentication methods: keyboard-interactive,
public key and password

	All utilities support SOCKS5 and HTTP proxies with
	user authentication support. This is handy if you're
	behind a firewall.

SSH2DOS is based on the WATT-32 TCP/IP library, Putty SSH client
for Windows, the ZLIB compression library and the CVT100
terminal emulation package.


## INSTALLATION

Unzip the package with subdirectory support (pkunzip -d).

If you have the binary package, no installation is needed.
Edit the wattcp.cfg and hosts file first, then install a packet
driver, or set up your PPP connection if you have a modem.

	To compile the sources, you'll also need the WATT-32 and ZLIB
	sources.
To build the binaries, build ZLIB and WATT-32 first. Be sure,
	that the WATT_ROOT environment variable points to the proper
WATT-32 source directory.
Copy zlib_f.lib or zlib_l.lib (depending on your target)
to the lib\ directory under the ssh2dos source tree.
	Now run 'make -f filename', where 'filename' is needed makefile:
	watcom_l.mak - OpenWatcom real mode target (for 8086 machines)
	watcom_f.mak - OpenWatcom protected mode target (for 80386 machines)

Tested compilers:
- OpenWatcom 1.x


## DOCUMENTATION

To get help for SSH2DOS and SFTPDOS, please use the /? command
line option.

-i <identity file> Public key file for public key authentication.
			You can create keys with PuTTYGen or Linux
			ssh-keygen.

-t <terminal type> This string is passed to the server as the
			'TERM' environment variable. The default is
			'xterm'. You can set any string here, but be
			sure to use a correct keymap file. For the
			nicest results, I recommend 'linux' with the
			linux keymap file, if your host supports it.

-p <port number>   Port to connect to at the remote host.
			The default SSH port is 22.

-k <keymap file>   Keymap file. Three sample keymap files are
			included in the package (for vt100/102,
			linux and xterm-color terminals). The 'xterm'
			keymap is hard-wired into SSH2DOS, so keymap
			files should only contain the differences
			from the 'xterm' keymap.

-m <mode>          Video mode. Valid modes are:
			'80x25', '80x60', '132x25' and '132x50'.
			A VESA VGA card is required for extended modes.

-s <password>      You can specify your password here. This is
			useful (but INSECURE) for batch files.

-l <log file>	   Log the whole session to a file.

-a <minutes>	   Send keepalive packets. SSH2DOS sends IGNORE
			packets in every 'minutes'.

-b <COM[1234]>     Copy all output to a Brailab PC adapter
			connected to the specified COM port.
			This adapter is useful for the visually
			challenged.

-g		   Use Diffie-Hellman group1 exchange. This may
			be useful in case of connection problems.

-P		   Use a non-privileged local port.

-C		   Enable compression (for real-mode SSH2DOS, you'll
			need as much free conventional memory as
			possible).

-S		   Suppress status line.

-B		   Use BIOS for screen writes (no direct video access).
						This may help visually challenged people.

-V		   Disable VESA BIOS. This can be useful to avoid
			changing to full-screen mode under WindowsXP
			or if you have other mode switching problems.

-n		   Add CR if server sends only LF. Use this to
			prevent the 'staircase effect'.

-d		   Save raw SSH packets to file 'debug.pkt'.

-v		   Be more verbose at startup.


During interactive session, SHIFT + PGUP/PGDOWN can be used
to view the scrollback area.

If the connection seems to be broken and no disconnection
happens automatically, you may terminate SSH2DOS by
pressing the ALT-X key combination.

DOS shell can be invoked using the ALT-E key combination.

To use proxy support, you must set either SOCKS_PROXY or
HTTP_PROXY environment variable. The syntax is:
	[username:password@]proxyhost[:port] 
The default SOCKS port is 1080, HTTP is 3128.
Some examples:
	SOCKS5 proxy with no user authentication using default port:
	SOCKS_PROXY=proxy.foo.bar
	HTTP proxy with authentication using port 8080:
	HTTP_PROXY=myusername:mypassword@proxy.foo.bar:8080


## EXAMPLES

Connect to a linux box:
	ssh2dos -t linux -k linux.kbd username hostname

Connect to a host using the 386 version and compression:
	ssh2d386 -C username hostname

Connect and run a shell script called 'scriptname':
	ssh2dos username hostname nohup scriptname &

Connect to a host with sftp 386 version:
	sftp386 username@hostname
Then use the 'help' command to get help.

Copy a file from the remote host to local:
	scpdos username@hostname:path_to_remote_file local_file

Create a batch file to upload to files, "file1" and "file2",
to a remote directory called "test" on "remotehost".
This batch file "example.txt" should look like:
	open username@remotehost
	cd test
	put file1
	put file2
	bye
Now use SFTP to do the actual transfer:
	sftpdos -b example.txt


## OFFICIAL SITE

The official distribution site is http://sshdos.sourceforge.net

The package is available in both executable and source format.
Contact: nagyd@users.sourceforge.net
