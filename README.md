OS X Framework build instructions
=================================

This git repository is used to build the OS X binary distribution of codyn. It
is supposed to live in a directory tree next to the codyn library, codyn-sharp
and rawc.

The compilation of the framework uses a local [homebrew] to compile the
dependencies (including autotools and glib). After compiling the dependencies,
codyn is compiled with specific options for building the framework. This basicly
means that some of the paths used at runtime will be hard coded to
/Library/Frameworks/Codyn.framework.

After compilation the framework can be built. A small python script constructs
the basic framework directory structure and copies all the files necessary into
the framework. References to dynamic libraries and include headers are
then automatically updated to refer to the right location.

When the framework is constructed, a package can be built using OS X
PackageMaker.

Prerequisites:
==============
1. Install Xcode.
   
   You will always need to install xcode before you can start to compile anything
   on OS X. Xcode can be installed easily from the App Store. Alternatively, you
   can try installing only the command line tools for Xcode from the apple
   developer download website ([Xcode command line tools][Xcode command line tools]).

2. Install git.
   
   git is required to fetch and use homebrew (used to compile the dependencies).
   You can get git packages for OS X from [git osx installer][git osx installer].

3. Get the codyn sources.
   
   You should create a directory with the following structure:
     * codyn/
     * codyn-sharp/
     * osx/ *(this repository)*
     * rawc/

   The sources can be either from [codyn git][codyn git] or from
   [codyn releases][codyn releases].

3. Bootstrap the dependencies.
   
   Run `scripts/bootstrap` to initialize homebrew and compile the dependencies.
   The resulting files will be installed in `osx/brew`.
   
   All the dependencies are compiled as universal binaries. Where possible,
   OS X system libraries (such as libxml2) are used.

3. Build codyn.
   
   Run `scripts/build` to build codyn. The resulting files will be installed in
   `osx/install`.

   This will compile codyn with the correct flags to make a universal
   dynamic library suitable for inclusion in an OS X framework.

4. Build the framework.
   
   Run `scripts/make-framework <version>` to build the codyn framework. The
   version has to be explicitly specified at the moment (i.e. this should be
   just the version of codyn that you compiled).

   This will result in a `osx/Codyn.framework` directory which contains the
   codyn binaries as a self-contained OS X framework.

5. Build the installer.
   
   Run `scripts/make-pkg <version>` to build the framework installer. After
   completion, the result package `osx/Codyn-<version>.pkg` should be
   available which installs the framework into
   /Library/Frameworks/Codyn.framework.

[homebrew]: https://github.com/mxcl/homebrew
[git osx installer]: http://code.google.com/p/git-osx-installer/downloads/list
[Xcode command line tools]: https://developer.apple.com/downloads/index.action?name=for%20Xcode%20-
[codyn git]: http://git.codyn.net
[codyn releases]: http://download.codyn.net/releases
