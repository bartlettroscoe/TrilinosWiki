Many users like to build Trilinos with warnings as errors.  With GCC and compatible compilers, this entails the `-Werror` flag.  (Trilinos has a CMake option for setting this in a compiler-independent way.  TODO: List the option here.)

The problem with building with warnings as errors, though, is that deprecated warnings break the build.  Trilinos users want to see deprecated warnings, so that they know they need to change their code.  However, they may not have time to fix their code right away.  This tempts them to either to turn off warnings as errors, or to disable deprecated warnings. 

GCC users can enable both warnings as errors _and_ deprecated warnings, while making deprecated warnings not count as errors.  Set this flag: `-Wno-error=deprecated-declarations`

For a brief discussion, see the following issue: https://github.com/trilinos/Trilinos/issues/1905