# Shaken Fist - Load Testing Tools
![Go](https://github.com/shakenfist/loadtest/workflows/Go/badge.svg) ![golangci-lint](https://github.com/shakenfist/loadtest/workflows/golangci-lint/badge.svg)

This is a home for tools used to load test a Shaken Fist cluster.

## CallCentre
Creates the requested number of instances and waits for each instance to call back thus confirming instance boot and network connectivity.

CallCentre starts a HTTP server listening for instance call backs. As each instance is started, a cloud-config file is inserted which commands a HTTP call to the CallCentre server.

Note that CallCentre should be run on a machine that allows network connections from the instances within Shaken Fist. (The easiest machine to use would be an actual Shaken Fist node.)

### Process
Call Centre will:
  1. Create a unique namespace
  2. Create a network within that namespace
  3. Start instances in the namespace
  4. Wait for each instance to call back via HTTP on port 8089
  5. When all instances have called back, delete all instances
  6. Delete the network
  7. Delete the namespace

### Cleanup
If the test fails or needs to be stopped, the `sf-client namespace clean` command can be used to clean up the instances and networks.

### Major options
|Flag  | Description
-------|:---------------
`--cpu`      | Number of CPU's in each instance
`--delCallback`  | Delete instance immediately after it calls back
`--delay`        | Delay between attempting to start instances (seconds, default=1)
`--ip`           | This servers reachable IP address from within the instances
`--load`         | Number of instances to start
`--memory`       | Memory size of each instance (in MB)

### Example
```
callcentre --load 5 --delay 3 --cpu 1 --memory 1024 --ip 192.168.72.201
```