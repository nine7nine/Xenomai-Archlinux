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

# Xenomai-cobalt

source package to build Xenomai userspace for Cobalt Core: dual-kernel configurations. This package builds both x86_64 / i686 versions of xenomai-cobalt/userspace. (for use on multilib/x86_64 systems). Xenomai userspace/libraries will be installed into;

* /usr/xenomai64
* /usr/xenomai32

You may want adjust your PATH, to include the /bin && /demo folders.

Please read through the Xenomai documentation, as this is a basic installation and you may want to tweak the package to your own requirements. Docs; https://xenomai.org/installing-xenomai-3-x/

NOTE: This packages depends on linux-xenomai and is useless without it.

# Xenomai-mercury

source package to build Xenomai userspace for Mercury Core: single-kernel configurations. Mercury Core/Xenomai only requires an -rt kernel (PREEMPT_RT_FULL) and doesn't patch the linux kernel for xenomai support, specifically. This package builds both x86_64 / i686 versions of xenomai-mercury/userspace. (for use on multilib/x86_64 systems). Xenomai userspace/libraries will be installed into;

* /usr/xenomai64
* /usr/xenomai32

(Again!) You may want adjust your PATH, to include the /bin && /demo folders.

Mercury Core will need an -rt kernel. You can install linux-rt, linux-rt-bfq or you can use my linux-rt_plus package (not found in AUR). It is located in my github, here; https://github.com/nine7nine/linux-rt-plus). however, I didn't make the xenomai-mercury depend on any particular kernel (mainly, because I sometimes use non-standard/local -rt packages). like I did with cobalt/linux-xenomai..

