# Kurtosis Tests for Pectra

## Installation

### Docker
Install Docker Desktop from [here](https://www.docker.com/products/docker-desktop)

Important: To run the tests Docker will need at least 14 GB of memory. You can change this in the Docker Desktop settings.

### Kurtosis
```
brew install kurtosis-tech/tap/kurtosis-cli
```

## Running the automatic tests
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

## Manual testing

While the network is started the following output should appear near the end of the output:
```
"pre_funded_accounts": [
    {
        "address": "0x8943545177806ED17B9F23F0a21ee5948eCaa776",
        "private_key": "bcdf20249abf0ed6d944c0288fad489e33f66b3960d9e6229c1cd214ed3bbe31"
    },

```

This is a list with accounts that already have some ETH. The private key can be used to import the account into a wallet or an Ethereum library.

To connect to the network search for user services that start with either Besu or Nethermind. It should look like this and be at the end of the output:
```
f0493f065732   el-2-nethermind-lighthouse                       engine-rpc: 8551/tcp -> 127.0.0.1:56642                RUNNING
                                                                metrics: 9001/tcp -> http://127.0.0.1:56643            
                                                                rpc: 8545/tcp -> 127.0.0.1:56645                       
                                                                tcp-discovery: 30303/tcp -> 127.0.0.1:56644            
                                                                udp-discovery: 30303/udp -> 127.0.0.1:61860            
                                                                ws: 8546/tcp -> 127.0.0.1:56646                        
8a2c15bde493   el-3-besu-nimbus                                 engine-rpc: 8551/tcp -> 127.0.0.1:56689                RUNNING
                                                                metrics: 9001/tcp -> http://127.0.0.1:56690            
                                                                rpc: 8545/tcp -> 127.0.0.1:56687                       
                                                                tcp-discovery: 30303/tcp -> 127.0.0.1:56691            
                                                                udp-discovery: 30303/udp -> 127.0.0.1:60114            
                                                                ws: 8546/tcp -> 127.0.0.1:56688                        

```
Port 8545 is the JSON-RPC port we need, it looks like this:
```
rpc: 8545/tcp -> 127.0.0.1:56645
```
The following example means that port 8545 of the client is mapped to `127.0.0.1:56645`. So we need to use this latter value as RPC URL in the wallet in order to connect to the network.

### Block explorer
To access the block explorer look for `blockscout` in the list of services. It should look like this:
```
732394dbb2e9   blockscout        http: 4000/tcp -> http://127.0.0.1:56750    RUNNING
```
In the example above `http://127.0.0.1:56750` needs to be used as the block explorer int the browser.

## Stopping the enclave

Once the tests are finished, you can stop the enclave with (will take a few minutes):
```
kurtosis enclave stop eth
```
