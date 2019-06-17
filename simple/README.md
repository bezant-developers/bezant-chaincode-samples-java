# `Java Chaincode` simple code
This tutorial will explain how to write `Hyperledger Fabric` chain code based on `Java`

# `Chaincode` development example

Writing your own chain code requires an understanding of the `Fabric` platform, `Java` and `Gradle`. An application is a basic example chain code that creates assets (key-value pairs) on a ledger.

``Put``
```bash
docker exec cli peer chaincode invoke -o orderer.example.com:7050 -C bezant-channel -n simple-java --peerAddresses peer0.bezant.example.com:7051 -c '{"Args":["put", "a", "10"]}'
```

``Get``
```bash
docker exec cli peer chaincode query -C bezant-channel -n simple-java --peerAddresses peer0.bezant.example.com:7051 -c '{"Args":["get", "a"]}'
```

``PutByeWalletAddress``
```bash
docker exec cli peer chaincode invoke -o orderer.example.com:7050 -C bezant-channel -n simple-java --peerAddresses peer0.bezant.example.com:7051 -c '{"Args":["putByWalletAddress", "a", "10"]}'
```

``Instantiate``
```bash
docker exec cli peer chaincode install -n simple-java -v 1.0 -l java -p /opt/gopath/src/simple-java
docker exec cli2 peer chaincode install -n simple-java -v 1.0 -l java -p /opt/gopath/src/simple-java                                                                                            
docker exec cli peer chaincode instantiate -o orderer.example.com:7050 -C bezant-channel -n simple-java -v 1.0 -c '{"Args":["init"]}'               
```

``Upgrade``
```bash
docker exec cli peer chaincode install -n simple-java -v 1.1 -l java -p /opt/gopath/src/simple-java
docker exec cli2 peer chaincode install -n simple-java -v 1.1 -l java -p /opt/gopath/src/simple-java                                                                                            
docker exec cli peer chaincode upgrade -o orderer.example.com:7050 -C bezant-channel -n simple-java -v 1.1 -c '{"Args":["init"]}'               
```