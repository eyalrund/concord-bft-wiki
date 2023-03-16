The Apollo framework provides utilities and advanced testing scenarios for validating
Concord BFT's correctness properties, regardless of the running application/execution engine.
For the purposes of system testing, we have implemented a "Simple Key-Value Blockchain" (SKVBC)
test application which runs on top of the Concord BFT consensus engine.
<br>

Apollo enables running all test suites (without modification) against any supported BFT network
configuration (in terms of <i>n</i>, <i>f</i>, <i>c</i> and other parameters).
<br>

Various crash or byzantine failure scenarios are also covered
(including faulty replicas and/or network partitioning).
<br>

Apollo test suites run regularly as part of Concord BFT's continuous integration pipeline.

Please find more details about the Apollo framework [here](https://github.com/vmware/concord-bft/blob/master/tests/apollo/README.md)

## Run examples


### Simple test application (4 replicas and 1 client on a single machine)

Tests are compiled into in the build directory and can be run from anywhere as
long as they aren't moved.

Run the following from the top level concord-bft directory:

   ./build/tests/simpleTest/scripts/testReplicasAndClient.sh

### Using simple test application via Python script

You can use the simpleTest.py script to run various configurations via a simple
command line interface.
Please find more information [here](https://github.com/vmware/concord-bft/blob/master/tests/simpleTest/README.md)

### Example application
This example demo application shows some capabilities of the Concord-BFT consensus-based byzantine fault-tolerant state machine replication library.
For Concord-BFT users who are interested in learning more about Concord-BFT and its uses, this application offers a demonstration and instruction.
Overall, any blockchain application based on concord-bft consensus may be created using this example application.
<br>

Use the [test_osexample.sh](https://github.com/vmware/concord-bft/blob/readme/examples/scripts/test_osexample.sh) script to run the example application. This script is also used to perform this demo via the command line interface with different configurations.
<br>

Please see [here](https://github.com/vmware/concord-bft/blob/master/examples/README.md) for more information about the examples/demo application.