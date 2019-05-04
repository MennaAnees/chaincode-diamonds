# chaincode-diamonds
project description:
creating a very simple chaincode which that will do the following:
      1-Allow for creation of a diamond asset
      2-Allow for transferring diamond ownership
      3-Allow for querying a diamond asset

How to run it:

1- Clone fabric-samples from github

2- Run the following command to clean up any instance of hyperledger (inside the ~/fabric-samples/first-network directory):
 ./teardown.sh

3-Create a new project folder for our chaincode:
mkdir -p ~/fabric-samples/chaincode/diamonds

4-Then, add the diamond.go file that uploaded here inside (~/fabric-samples/chaincode/diamonds) folder

5-Go to the sample network script repository:
cd ~/fabric-samples/basic-network

6-Launch a sample Hyperledger Fabric instance with the following command:
docker-compose up -d orderer.example.com peer0.org1.example.com cli

7-Launch two additional network setup tasks that will create a sample channel called mychannel:

docker exec -e "CORE_PEER_LOCALMSPID=Org1MSP" -e "CORE_PEER_MSPCONFIGPATH=/etc/hyperledger/msp/users/Admin@org1.example.com/msp" peer0.org1.example.com peer channel create -o orderer.example.com:7050 -c mychannel -f /etc/hyperledger/configtx/channel.tx

8-Now add an existing peer to it:

docker exec -e "CORE_PEER_LOCALMSPID=Org1MSP" -e "CORE_PEER_MSPCONFIGPATH=/etc/hyperledger/msp/users/Admin@org1.example.com/msp" peer0.org1.example.com peer channel join -b mychannel.block

9- Install Chaincode:

docker exec -e "CORE_PEER_LOCALMSPID=Org1MSP" -e "CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp" cli peer chaincode install -n diamonds -v 1.0 -p "github.com/diamonds" -l "golang"

10- Instantiate the Chaincode:

docker exec -e "CORE_PEER_LOCALMSPID=Org1MSP" -e "CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp" cli peer chaincode instantiate -o orderer.example.com:7050 -C mychannel -n diamonds -l "golang" -v 1.0 -c '{"Args":[""]}' -P "OR ('Org1MSP.member')"

11- create a diamond its name is "Kohinoor" and its origin is "India" and its carats weights 105:

docker exec -e "CORE_PEER_LOCALMSPID=Org1MSP" -e "CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp" cli peer chaincode invoke -o orderer.example.com:7050 -C mychannel -n diamonds -c '{"Args":["createDiamond","Kohinoor","India","105","Albert"]}'


12- query the diamond called "Kohinoor":

docker exec -e "CORE_PEER_LOCALMSPID=Org1MSP" -e "CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp" cli peer chaincode query -C mychannel -n diamonds -c '{"Args":["queryDiamond","Kohinoor"]}'

13- transfer the diamond to an owner called "Victoria"

docker exec -e "CORE_PEER_LOCALMSPID=Org1MSP" -e "CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp" cli peer chaincode invoke -o orderer.example.com:7050 -C mychannel -n diamonds -c '{"Args":["transferDiamond","Kohinoor","Victoria"]}'
