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
make must be run afterwards to build according to the configuration. Please see [CMakeLists.txt](CMakeLists.txt) for configurable options.

#### Select Communication Module
One option that is worth calling out explicitly is the communication (transport) library.

#### Select Communication Module
One option that is worth calling out explicitly is the communication (transport) library. Transport defaults to TLS and can be configured explicitly by setting the `CONCORD_BFT_CMAKE_TRANSPORT` flag. The flag defaults to **TLS**, but also supports **UDP** and **TCP**. These can be useful because the use of pinned certificates for TLS requires an out of band setup.
As we used pinned certificates for TLS, the user will have to manually provide these. They can use the [create_tls_certs.sh](scripts/linux/create_tls_certs.sh) script as an example.

//EEE is the default different for native?
We support both UDP and TCP communication. UDP is the default. In order to
enable TCP communication, build with `-DBUILD_COMM_TCP_PLAIN=TRUE` in the cmake
instructions shown above.  If set, the test client will run using TCP. If you
wish to use TCP in your application, you need to build the TCP module as
mentioned above and then create the communication object using CommFactory and
passing PlainTcpConfig object to it.

