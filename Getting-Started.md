## Getting the Source

git clone https://github.com/vmware/concord-bft.git

## Install and Build (Ubuntu Linux 18.04)

Concord-BFT supports two kinds of builds: native and docker.

The docker build is **strongly recommended**.

### Docker

* Install the latest docker.
* Optional: [configure docker as non-root user](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user).
* Build:
```sh
cd concord-bft
make
make test
```
Run `make help` to see more commands.

Note:
* The output binaries are stored in the host's `concord-bft/build`.
* `Makefile` is configurable.
For example, if you want to use another compiler you may pass it to the `make`:
```
make CONCORD_BFT_CONTAINER_CXX=g++ \
    CONCORD_BFT_CONTAINER_CC=gcc \
    build
```

Other build options, including passthrough options for CMake, are defined in the Makefile and prefixed with `CONCORD_BFT_`. Variables that are capable of being overridden on the command line are set with the Make conditional operator `?=` and are at the beginning of [Makefile](https://github.com/vmware/concord-bft/blob/master/Makefile). Please check that file for options.

### Native Build

```sh
cd concord-bft
sudo ./install_deps_release.sh # Installs all dependencies and 3rd parties
mkdir build
cd build
cmake ..
make
sudo make test
```

By default the build chooses the active compiler on the native build platform. In order to force compilation by clang you can use the following command.
```sh
CC=clang CXX=clang++ cmake ..
```

In order to turn on or off various options, you need to change your cmake configuration. This is
done by passing arguments to cmake with a `-D` prefix: e.g. `cmake -DBUILD_TESTING=OFF`. Note that
make must be run afterwards to build according to the configuration. Please see [CMakeLists.txt](https://github.com/vmware/concord-bft/blob/master/CMakeLists.txt) for configurable options.

#### Select Communication Module
One option that is worth calling out explicitly is the communication (transport) library. Transport defaults to TLS and can be configured explicitly by setting the `CONCORD_BFT_CMAKE_TRANSPORT` flag. The flag defaults to **TLS**, but also supports **UDP** and **TCP**. These can be useful because the use of pinned certificates for TLS requires an out of band setup.
As we used pinned certificates for TLS, the user will have to manually provide these. They can use the [create_tls_certs.sh](https://github.com/vmware/concord-bft/blob/master/scripts/linux/create_tls_certs.sh) script as an example.

### C++ Linter

The C++ code is statically checked by `clang-tidy` as part of the [CI](https://github.com/vmware/concord-bft/actions?query=workflow%3Aclang-tidy).
<br>To check code before submitting PR, please run `make tidy-check`.

[Detailed information about clang-tidy checks](https://clang.llvm.org/extra/clang-tidy/checks/list.html).

#### (Optional) Python client

The python client is required for running tests. If you do not want to install python, you can
configure the build of concord-bft by running `cmake -DBUILD_TESTING=OFF ..` from the `build`
directory for native builds, and `CONCORD_BFT_CMAKE_BUILD_TESTING=TRUE make` for docker builds.

The python client requires python3(>= 3.5) and trio, which is installed via pip.

    python3 -m pip install --upgrade trio

#### Adding a new dependency or tool

The CI builds and runs tests in a docker container. To add a new dependency or tool, follow the steps below:

* Rebase against master
* In order to add/remove dependencies update the file
  [install_deps_release.sh](https://github.com/vmware/concord-bft/blob/master/install_deps_release.sh)
* Build new release/debug images: `make build-docker-images`
* Check images current version in
  [Makefile](https://github.com/vmware/concord-bft/blob/master/Makefile#L5)
  and
  [Makefile](https://github.com/vmware/concord-bft/blob/master/Makefile#L10)
* Tag the new images (at least one):
  * For release: `docker tag concord-bft:latest concordbft/concord-bft:<version>`
  * For debug: `docker tag concord-bft-debug:latest concordbft/concord-bft-debug:<version>`
  <br>where version is `current version + 1`.
* Update the version in the Makefile
* Make sure that Concord-BFT is built and tests pass with the new images: `RUN_WITH_DEBUG_IMAGE=TRUE make
  clean build test`
* Ask one of the maintainers for a temporary write permission to Docker Hub
  repository(you need to have a [Docker ID](https://docs.docker.com/docker-id/))
* Push the images (at least one):
  * For release: `docker push concordbft/concord-bft:<version>`
  * For debug: `docker push concordbft/concord-bft-debug:<version>`
* Create a PR for the update:
    * The PR must contain only changes related to the updates in the image
    * PR's summary has to be similar to `Docker update to version release=<new version> debug=<new version>`
    * PR's message has to list the changes made in the image content and
      preferably the reason
    * Submit the PR

Important notes:
1. Adding dependencies or tools directly to the `Dockerfile` is strongly not recommended because it breaks the native build support.
2. If any tools are installed during the build but not needed for the actual compilation/debugging/test execution(for example, `git`), please remove them(`Dockerfile` is an example). The reason is that the image is supposed to be as tiny as possible.

//EEE is the default different for native?
We support both UDP and TCP communication. UDP is the default. In order to
enable TCP communication, build with `-DBUILD_COMM_TCP_PLAIN=TRUE` in the cmake
instructions shown above.  If set, the test client will run using TCP. If you
wish to use TCP in your application, you need to build the TCP module as
mentioned above and then create the communication object using CommFactory and
passing PlainTcpConfig object to it.

