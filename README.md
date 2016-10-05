# Xenomai-Archlinux

this repo contains various Archlinux source packages for Xenomai-3.0.3. 

What is Xenomai about?

Xenomai is about making various real-time operating system APIs available to Linux-based platforms. When the target Linux kernel cannot meet the requirements with respect to response time constraints, Xenomai can also supplement it for delivering stringent real-time guarantees.

The project has a strong focus on embedded systems, although Xenomai runs over mainline desktop and server architectures as well.

To sum up, Xenomai can help you in:

    designing, developing and running a real-time application on Linux.
    migrating an application from a proprietary RTOS to Linux.
    optimally running RTOS applications (VxWorks, pSOS, VRTX, uITRON, POSIX) alongside native Linux applications.

For more information on Xenomai, go here; https://xenomai.org/start-here/

# Xenomai Packages:

# linux-xenomai: 

source package to build a cobalt/xenomai linux kernel. This is the 'dual-kernel' approach to providing RT, used in Xenomai.
Linux-xenomai must be used with the 'xenomai-cobalt' source package. (more on that below). For convenience, I have added a repo/kernel sources for this package; https://github.com/nine7nine/linux-4.1.18-xenomai ... The PKGBUILD will grab this 'pre-patched' xenomai/cobalt dual-kernel, avoiding having to manually step through applying the cobalt/ipipe patchwork.

Additionally, my source package enables gcc optimizations and adds support for the BFQ io scheduler.

That all said, you should still be reading through the documentation, as there is important information. For example, there are a number of different kernel parameters, etc; https://xenomai.org/installing-xenomai-3-x/

IMPORTNAT NOTES: 

1. This kernel config adheres to Xenomai's recommended configs. Therefore, hyperthreading, acpi, intel_idle, cpurfreq, etc have all been disabled in my config for this kernel. That all being said; some of the benchmarks I have done are VERY impressive. Cyclictest reports extremely low latencies, with very little jitter. It should be noted though that apps need to make use of the Cobalt Core to really benefit. (read on for more info / read Xenomai's documentation).

2. I haven't enabled every driver / feature, so you might want to run make menuconfig/xconfig to see if there is a missing feature and/or driver that you will require. however, most features and debugging have been enabled.

# Xenomai-cobalt

source package to build Xenomai userspace for Cobalt Core: dual-kernel configurations. This package builds both x86_64 / i686 versions of xenomai-cobalt/userspace. (for use on multilib/x86_64 systems). Xenomai userspace/libraries will be installed into;

* /usr/xenomai/cobalt64
* /usr/xenomai/cobalt32

Please read through the Xenomai documentation, as this is a basic installation and you may want to tweak the package to your own requirements. Documentation; https://xenomai.org/installing-xenomai-3-x/

NOTE: This packages depends on linux-xenomai and is useless without it.

# Xenomai-mercury

source package to build Xenomai userspace for Mercury Core: single-kernel configurations. Mercury Core/Xenomai only requires an -rt kernel (PREEMPT_RT_FULL) and doesn't patch the linux kernel for xenomai support, specifically. This package builds both x86_64 / i686 versions of xenomai-mercury/userspace. (for use on multilib/x86_64 systems). Xenomai userspace/libraries will be installed into;

* /usr/xenomai/mercury64
* /usr/xenomai/mercury32

Mercury Core will need an -rt kernel. You can install linux-rt, linux-rt-bfq or you can use my linux-rt_plus package (not found in AUR). It is located in my github, here; https://github.com/nine7nine/linux-rt-plus). 

# Compatibility

You may have noticed from the above installation PATHs that Mercury and Cobalt Core/Userspace can be installed/co-exist on 
the same system. (yes, the packages won't conflict!)... This can be very useful for testing/comparison purposes. For example,
you can switch kernels (linux-xenomai or linux-rt/PREEMPT_RT) and if you have xenomai-cobalt && xenomai-mercury both installed,
you will have access to the correct Xenomai Userspace && libraries.

Next step; if you happen to have apps correctly built against each Mercury/Cobalt Core; doing comparisions is a fairly
trivial task (ie: run the correct binary, userspace and kernel together).

Please consult the documentation on building Xenomai apps; 

* https://xenomai.org/building-applications-with-xenomai-3-x/
* https://xenomai.org//2015/05/application-setup-and-init/

NOTE: Since there are multiple xenomai core installations, you must call the correct xeno-config, when building apps. Following Xenomai's documentation on building apps (makefile example), you would substitute the correct XENO_CONFIG path;

COBALT CORE (64 or 32 bit) && linux-4.1.18-xenomai

* XENO_CONFIG := /usr/xenomai/cobalt64/bin/xeno-config
* XENO_CONFIG := /usr/xenomai/cobalt32/bin/xeno-config

MERCURY CORE (64 or 32 bit) && PREEMPT_RT_FULL kernel (linux-rt)

* XENO_CONFIG := /usr/xenomai/mercury64/bin/xeno-config
* XENO_CONFIG := /usr/xenomai/mercury32/bin/xeno-config

Before building the app, you should export the correct PATH (cobalt 64bit example);

* export PATH=/usr/xenomai/cobalt64/bin:$PATH

(then run ./configure, make, etc)

If you are testing/building against each core (mercury/cobalt) with the same app/program,  then you will need to install them to different locations. However, in some cases (like single binaries) you may be able to just add a suffix. ie; 

* /usr/bin/foo_cobalt
* /usr/bin/foo_mercury

This configuration (imho) gives some flexibility and the installation of my Xenomai packages doesn't require
special magic or any extra effort to support this ~ Just build your apps against the correct target, then boot between 
kernels and test your apps / configurations... 
