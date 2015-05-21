openssh-knockd.patch
====================

Introduction
------------

It's just a small patch to integrate the portknock in the SSH client.


Patching the source
-------------------

Install the **knockd** package:

	deftcode ~ $ sudo apt-get install knockd

Download the SSH portable source from: http://ftp.eu.openbsd.org/pub/OpenBSD/OpenSSH/portable/openssh-6.8p1.tar.gz

Now apply the **openssh-knockd.patch** to the source file **ssh.c**:

	deftcode openssh-knockd > ls
	openssh-6.8p1  openssh-6.8p1.tar.gz  openssh-knockd.patch  README.md
	osiride openssh-knockd > patch openssh-6.8p1/ssh.c < openssh-knockd.patch 
	patching file openssh-6.8p1/ssh.c
	osiride openssh-6.8p1 > sudo ./configure && make && make install
	checking for gcc... gcc
	checking for C compiler default output file name... a.out
	...

The patch will read the **/etc/knockd.conf** and will search the **openSSH** sequence that will be need for enable 
the SSH service to the allowed ip address when a  SSH handshake gets started.

Test the openssh-knockd patch
-----------------------------

The following command execute the knockd sequence request to enable SSH service to the allowed ip address:
(The root permissions are needed just to read the **/etc/knockd.conf** configuration file)

	deftcode openssh-6.8p1 > sudo ./ssh -j eurialo@deftcode.local
	portknock: sending tcp sequence on port 9000.
	portknock: sending tcp sequence on port 8000.
	portknock: sending tcp sequence on port 7000.
	eurialo@deftcode.local's password: 

With this patch you don't need to run before each connection, the knock request, eg:

	deftcode ~ $ knock -v deftcode.local [port1] [port2] [port3]
