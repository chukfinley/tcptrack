### fixed bug in src/TextUI.cc
lines are 315, 317, and 319.
```
		if( ic->getIdleSeconds() < 60 )
			printw("%lds",ic->getIdleSeconds());
		else if( ic->getIdleSeconds() < 3600 ) 
			printw("%ldm",ic->getIdleSeconds()/60);
		else
			printw("%ldh",ic->getIdleSeconds()/3600);
```
file is updated just do 
```
git clone https://github.com/chukfinley/tcptrack/
cd tcptrack/
./configure
make
sudo make install
```


Getting Started
---------------

	See the INSTALL file for instructions on how to build and install
	and start using tcptrack.


Known Bugs / Gotchas
--------------------

	tcptrack does not compile cleanly with libpcap versions earlier than
	0.7.2. See below for a hack to your libpcap <0.7.2 to get tcptrack
	to compile on it, or upgrade your libpcap.

	On some systems at the end of the build process (after doing make),
	you may see "g++: unrecognized option -pthread". This is a problem
	with the configure script thinking that your c++ compiler actually
	accepts this option. This message isn't actually a fatal error.
	Unless some other error occurred, you should have a working tcptrack
	binary built and can test it or proceed to 'make install'.

	I have had one report that tcptrack will fail to compile on Solaris
	giving a ton of errors involving stl include files while doing
	'make'. I have not been able to reproduce this on any Solaris
	machine that I have access to. I'm not sure if there's a problem in
	tcptrack or elsewhere. If you encounter this, please let me know.


Notes
-----

	Compilation may fail with errors about multiple declarations of
	bpf_validate and bpf_filter.  There is a bug in libpcap versions
	earlier than 0.7.2 which causes this to happen when using libpcap in
	C++ programs. To correct it get libpcap 0.7.2 or higher from
	www.tcpdump.org or see the notes at the end of this README file on
	how to fix your version.

	On Redhat 7.x configure or compilation may fail with errors about
	net/bpf.h not existing, or configure may tell you that it cannot
	find pcap.h when you in fact have it. I think this is a bug with
	Redhat's libpcap packages. See below for instructions on how to fix
	it, or install libpcap >= 0.7.2 from source.


Fixing Redhat pcap build/configure problems
-------------------------------------------

	As mentioned above in the Notes section, this may be needed if
	you're on Redhat 7.x and configure tells you you don't have pcap.h
	when in fact you do.

	Edit your pcap.h file (/usr/include/pcap/pcap.h on RH 7.x).

	Line 42 reads:

		#include <net/bpf.h>
	
	Change it to read:

		#include <pcap/net/bpf.h>

	Now try running configure again. If you get 'previous declaration'
	errors while running make, see the next section.
	

Fixing C++ bug in pcap < 0.7.2
------------------------------
	
	This is a hack that is only needed if you're experiening the build
	problems about multiple declarations of bpf_validate and bpf_filter
	mentioned above in the Notes section. Note that this may cause other
	programs to fail to compile, so you should change it back after
	you've built tcptrack (or upgrade your libpcap if posssible).

	Edit your pcap.h. It may be in /usr/include or /usr/include/pcap.

	Around line 169 (as of libpcap 0.6.2, YMMV) you should see the
	following lines:

		u_int bpf_filter(struct bpf_insn *, u_char *, u_int, u_int);
		int     bpf_validate(struct bpf_insn *f, int len);	

	Comment them out:

		/*
		u_int bpf_filter(struct bpf_insn *, u_char *, u_int, u_int);
		int     bpf_validate(struct bpf_insn *f, int len);	
		*/

	Now try building again.


Reporting Bugs / Comments / Requests / Patches
----------------------------------------------

	Send me (Steve Benson) an email at
	steve@rhythm.REMOVE_THIS_SPAMTRAP.cx if you have questions or find
	bugs or anything.

