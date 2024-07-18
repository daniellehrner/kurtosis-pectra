# Kurtosis Tests for Pectra

## Installation

### Docker
Install Docker Desktop from [here](https://www.docker.com/products/docker-desktop)

Important: To run the tests Docker will need at least 14 GB of memory. You can change this in the Docker Desktop settings.

### Kurtosis
```
brew install kurtosis-tech/tap/kurtosis-cli
```

## Running the tests
Check that if the `eth` enclave already exists:
```
kurtosis enclave ls
```

If it does, remove it with:
```
kurtosis enclave rm -f eth
```

Start the tests with:
```
kurtosis run --enclave eth github.com/ethpandaops/ethereum-package --args-file network_params.yaml
```

The end of the created output will look similar to this:
```
========================================== User Services ==========================================
UUID           Name                                             Ports                                         Status
d89066cbd3cf   apache                                           http: 80/tcp -> http://127.0.0.1:56615        RUNNING
3750fca05605   assertoor                                        http: 8080/tcp -> http://127.0.0.1:56616      RUNNING
4b5957123a58   cl-1-lighthouse-nethermind                       http: 4000/tcp -> http://127.0.0.1:56518      RUNNING
                                                                metrics: 5054/tcp -> http://127.0.0.1:56519   
                                                                tcp-discovery: 9000/tcp -> 127.0.0.1:56520    
                                                                udp-discovery: 9000/udp -> 127.0.0.1:55649    
daff4b0c75b4   cl-2-teku-nethermind                             http: 4000/tcp -> http://127.0.0.1:56528      RUNNING
                                                                metrics: 8008/tcp -> http://127.0.0.1:56526   
```

Look for the `assertoor` service and enter its URL in your browser to see the test results. In the example above it is `http://127.0.0.1:56616`.

Once the tests are finished, you can stop the enclave with (will take a few minutes):
```
kurtosis enclave stop eth
```
