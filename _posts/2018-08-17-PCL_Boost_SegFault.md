---
layout: default
title: PCL/Boost Segmentation Fault 
date:  2018-08-17
category: [ Linux, C++, PCL,Boost]
---

<p>
I was recently trying to use PCL's convex hull functions and even though I had successfully ran it before this time it didn't work. The thing is, it compiles successfully but seg faults immediately on startup. As I was editing a large library, I didn't know what was going on. Anyway it turns out, this is a well know issue with PCL documented 
<a href="https://github.com/ros-industrial-consortium/CAD-to-ROS/pull/96">here </a>,
<a href="https://github.com/PointCloudLibrary/pcl/issues/780"> here </a>, 
<a href="https://github.com/felixendres/rgbdslam_v2/issues/8"> here </a>,
<a href="https://github.com/PointCloudLibrary/pcl/issues/619"> here </a>, and
<a href="https://stackoverflow.com/questions/39292457/c-segmentation-fault-in-empty-main-in-a-non-empty-project"> here </a>. The accepted solution seems to be either don't use C++11 or recompile PCL with C++11. It seems the issue comes from different compiler versions in the boost libraries. I eventually got round the problem by adding a a few tags in the CMakelists file (I think one of the git issues said as much).
</p>

<h3> 1. My System information</h3>
<li>Ubuntu 14.04 </li>
<li>Boost version: 1.54.0</li>
<li>PCL 1.7</li>
<li>The CXX compiler identification is GNU 4.8.4</li>


<h3> 2. Problemn</h3>
<p>
Running my ros node with as follows
</p>

<code>
rosrun --prefix 'gdb --args' <package_name> <node_name> 
</code>
  
 <p>
 yielded
</p>

```
Program received signal SIGSEGV, Segmentation fault.
0x00007ffff3453ae0 in boost::math::lanczos::lanczos_initializer<boost::math::lanczos::lanczos17m64, long double>::init::init() () from /usr/lib/libpcl_sample_consensus.so.1.7


Program received signal SIGSEGV, Segmentation fault.
0x00007ffff3453ae0 in boost::math::lanczos::lanczos_initializer<boost::math::lanczos::lanczos17m64, long double>::init::init() () from /usr/lib/libpcl_sample_consensus.so.1.7
(gdb) bt
#0  0x00007ffff3453ae0 in boost::math::lanczos::lanczos_initializer<boost::math::lanczos::lanczos17m64, long double>::init::init() () from /usr/lib/libpcl_sample_consensus.so.1.7
#1  0x00007ffff342b5ce in ?? () from /usr/lib/libpcl_sample_consensus.so.1.7
#2  0x00007ffff7dea1da in call_init (l=<optimized out>, argc=argc@entry=1, argv=argv@entry=0x7fffffffcf18, env=env@entry=0x7fffffffcf28) at dl-init.c:78
#3  0x00007ffff7dea2c3 in call_init (env=<optimized out>, argv=<optimized out>, argc=<optimized out>, l=<optimized out>) at dl-init.c:36
#4  _dl_init (main_map=0x7ffff7ffe1c8, argc=1, argv=0x7fffffffcf18, env=0x7fffffffcf28) at dl-init.c:126
#5  0x00007ffff7ddb29a in _dl_start_user () from /lib64/ld-linux-x86-64.so.2
#6  0x0000000000000001 in ?? ()
#7  0x00007fffffffd380 in ?? ()
#8  0x0000000000000000 in ?? ()
```

<h3> 3. Work-around</h3>
 <p>
To resolve the issue the following tags were put in the CMakelists
</p>


```
  
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  
  message(STATUS "Setting build type to 'Release' as none was specified.")
  
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build." FORCE)
  
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" 
    "MinSizeRel" "RelWithDebInfo")
   
endif()
```

 <p>
  And voilà! 4 hours of my life I'm never getting back. 
  </p>
