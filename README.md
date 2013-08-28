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

3. Install python3
...
...Python3 should be installed from the latest [python3 release][python3 release]
   to support the pygobject bindings for python3. This is mainly needed for the
   blender support.

4. Install the Mono framework
...
...The mono framework is required to install rawc and the studio. The mono
...framework installer can be obtained from the [mono website][mono website].

5. Bootstrap the dependencies.
   
   Run `scripts/bootstrap` to initialize homebrew and compile the dependencies.
   The resulting files will be installed in `osx/.brew` and `osx/.brewbuild`.
   This will also fetch all the codyn sources needed to build the framework
   in `.sources/`.
   
   All the dependencies are compiled as universal binaries. Where possible,
   OS X system libraries (such as libxml2) are used.

6. Build codyn.
   
   Run `scripts/build` to build codyn. The resulting files will be installed in
   `osx/.install` and `osx/.install-mono`.

   This will compile codyn with the correct flags to make a universal
   dynamic library suitable for inclusion in an OS X framework. At the moment
   two versions of the codyn library will be built. The first is a universal
   binary linked against bundled dynamic libraries of glib, gobject etc. This
   library can be used when developing an application against codyn. The second
   library is built in `osx/.install-mono` and is only used in combination with
   the C# bindings (needed by rawc and the studio). This is needed currently
   because mono ships with its own version of glib/gobject and mono is only
   available in 32 bits at this moment.
   
   When available, codyn-sharp and rawc are also built and installed in
   `osx/.install-mono` and will be bundled in the framework.

7. Build the framework.
   
   Run `scripts/make-framework [<version>]` to build the codyn framework. The
   version specified is optional and when omitted is extracted from
   `codyn/configure.ac`.

   This will result in a `osx/Codyn.framework` directory which contains the
   codyn binaries as a self-contained OS X framework. It will also provide
   all the necessary setup and bundled libraries to run rawc using mono.

8. Build the installer.
   
   Run `scripts/make-pkg <version>` to build the framework installer. After
   completion, the result package `osx/Codyn-<version>.pkg` should be
   available which installs the framework into
   /Library/Frameworks/Codyn.framework. The package simply installs the
   framework in /Library/Frameworks and creates some symlinks for the cdn-
   binaries in /usr/bin.

[homebrew]: https://github.com/mxcl/homebrew
[git osx installer]: http://code.google.com/p/git-osx-installer/downloads/list
[Xcode command line tools]: https://developer.apple.com/downloads/index.action?name=for%20Xcode%20-
[codyn git]: http://git.codyn.net
[codyn releases]: http://download.codyn.net/releases
[python3 release]: http://www.python.org/getit/
[mono website]: http://www.go-mono.com/mono-downloads/
