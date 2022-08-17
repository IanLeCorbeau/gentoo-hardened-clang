# gentoo-hardened-clang

Config for a hardened Gentoo system built on top of the Clang profile.

***This is a work in progress***

Usage
-----

To test this out, start from scratch following the [Gentoo handbook](https://wiki.gentoo.org/wiki/Special:MyLanguage/Handbook:AMD64), and at install time:

* Choose the stage3-amd64-clang-openrc tarball.
* Before the mirror select step, copy the contents of __/etc/portage/__ to the system's portage dir.  
  * Make sure to inspect them all before proceeding. This does more than hardening the Clang toolchain. 
  * You may wish to remove __package.use/mandoc__ and __package.accept_keywords/mandoc__ and remove the _PORTAGE_COMPRESS_ lines in __make.conf__ if you do not want to use mandoc as the system man.
  * In __make.conf__, you may wish to adjust MAKEOPTS="-j$(nproc)" if you want to keep a cpu core free during compile time.
* If you want the full base system that this repository provides, [add](https://wiki.gentoo.org/wiki/World_file_(Portage)#Adding_an_atom_without_recompilation) the contents of __/var/lib/portage/world__ to the system's @world file.
* When you're at the stage of emerging/updating @world for the first time, brace yourself.

LLVM_TARGETS
------------

Unfortunately, Gentoo currently forces all LLVM targets to be enabled at build time, which triples LLVM and Clang's compile time. This is a necessay evil due to a bug which causes an issue with reverse dependencies (e.g. Rust) when some targets are disabled. It is possible to disable the unnecessary targets by uncommenting them in /etc/portage/profile/package.use.force, as long as you understand that you'll probably have to build all targets anyway at some point (provided the issue hasn't been fixed by then).

I can only confirm that the packages in the base system and the [@world](https://github.com/I-LeCorbeau/gentoo-hardened-clang/blob/main/var/lib/portage/world) file build perfectly fine with targets disabled by force. 
  
TODO
----

- [ ] Provide a hardened kernel config 
- [ ] Remove multilib support  
- [ ] Switch to Musl

