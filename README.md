# jvmti


JVMTI Demonstration Code

The JavaTM Virtual Machine Tools Interface (JVMTI) is a native tool interface provided in v1.5. Native libraries that use JVMTI and are loaded into the JavaTM Virtual Machine via the -agentlib, -agentpath, or -Xrun (deprecated) interfaces, are called Agents.

JVMTI was designed to work with the JavaTM Native Interface (JNI), and eventually displace the JavaTM Virtual Machine Debugging Interface (JVMDI) and the JavaTM Virtual Machine Profiling Interface (JVMPI).

We have tried to create a set of demonstration agents that should help demonstrate many of the features and abilities of the interface. This list of demonstration agents will change over time. They are provided as educational tools and as starting points for JavaTM tool development.
Agents

    versionCheck
    This is a extremely small agent that does nothing but check the version string supplied in the jvmti.h file, with the version number supplied by the VM at runtime.
    mtrace
    This is a small agent that does method tracing. It uses Bytecode Instrumentation (BCI) via the java_crw_demo library.
    gctest
    This is a small agent that does garbage collection counting.
    heapViewer
    This is a small agent that does some basic heap inspections.
    heapTracker
    This is a small agent that does BCI to capture object creation and track them. It uses Bytecode Instrumentation (BCI) via the java_crw_demo library.
    waiters
    This is a small agent that gets information about threads waiting on monitors.
    hprof
    This is a large agent that does heap and cpu profiling. This demo agent is actually built into the JavaTM Runtime Environment (JRE). It uses Bytecode Instrumentation (BCI) via the java_crw_demo library.
    Note: hprof is NOT a small or simple agent, the other smaller demos should be looked at first.

Agent Support

    java_crw_demo
    This is a demo C library that does class file to class file transformations or BCI (Bytecode Instrumentation). It is used by several of the above agents.

Native Library Build Hints

All libraries loaded into java are assumed to be MT-safe (Multi-thread safe). This means that multiple threads could be executing the code at the same time, and static or global data may need to be placed in critical sections. See the Raw Monitor interfaces for more information.

All native libraries loaded into the JavaTM Virtual Machine, including Agent libraries, need to be compiled and built in a compatible way. Certain native compilation options or optimizations should be avoided, and some are required. More information on this options is available in the man pages for the various compilers.

    On Solaris and using the Sun C/C++ Compilers 5.5 (Studio 8 Compiler Collection):
        Required: On SPARC, -xarch=v8 or -xarch=v9
        This is to be specific as to the sparc architecture. Be careful when using -xarch and some of the other compiler optimization options that target specific processors, you can easily limit the types of machines the library will run on, e.g. using -xarch=v9b will limit you to UltraSPARC III capable processor.
        Required: -mt
        This should be used for compiles and links, it makes sure the library is built properly to be a MT-safe library.
        Required: On SPARC, -xregs=no%appl.
        Certain registers should not be used by system libraries.
        Recommended: No higher than -xO2 on optimization.
        It is important that these libraries have frame pointer register usage. Optimizations above -xO2 can remove the use of frame pointer registers and cause problems when doing stack frame walking.
        Recommended: ldd -r LibraryName
        After the shared library has been built, the utility 'ldd' (or 'ldd -r') can be used to verify that all dependent libraries have been satisfied, and all externs can be found. If 'ldd' says anything is missing, it is very likely that the JVM will be unable to load this library.
    On Linux (Using the gcc version 3.2):
        Required: -fno-omit-frame-pointer
        It is important that these libraries have frame pointer register usage.
        Recommended: -fno-strict-aliasing
        Avoid this pointer optimization.
        Required: -Wl,-soname=LibraryName
        When building the shared library (-shared option), this option makes sure the library name is stored inside the library.
        Required: -static-libgcc
        When building the shared library (-shared option), this option allows for maximum portability of the library between different flavors of Linux.
        Recommended: ldd -r LibraryName
        Provides the same checking as Solaris (see above).
    On Windows and using the Microsoft C++ Compiler:
        Recommended: -Op.
        Avoid optimizations that change floating point precision.
        Required: -Gy.
        Allow for function level linking.
        Recommended: VC++ dumpbin/exports and VC++ "Dependency Walker"
        Provides dependency information similar to ldd.

For More Information

For more detailed information on JVMTI, refer to http://java.sun.com/j2se/1.5.0/docs/guide/jvmti.

More information on using JNI and building native libraries refer to: http://java.sun.com/j2se/1.5.0/docs/guide/jni.

Additional information can also be found by doing a search on "JVMTI" at http://java.sun.com/j2se. Various technical articles are also available through this website. And don't forget the JavaTM Tutorials at http://java.sun.com/docs/books/tutorial for getting a quick start on all the various interfaces.
Comments and Feedback

Comments regarding JVMTI or on any of these demonstrations should be sent through http://java.sun.com/mail/ 