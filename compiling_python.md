# Compiling Python from source
**NOTE:** Assuming we are on Fedora, if on Redhat use yum instead of dnf.

1. Ensure all the build requirements are installed, see [this page](https://devguide.python.org/). Easiest way to do this is to use dnf-plugins-core which is a dnf add-on. Then run the following to automagically installed all system libs to build Python
    1. `sudo dnf builddep python3`
1. Download and extract the source
1. CD into the source folder
1. `./configure --prefix=/home/brett/myPython --enable-optimizations --with-lto`
1. `make`
1. `make test`
1. `make install`
