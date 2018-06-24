# Social Care User Registry with Hyperledger Blockchain Technology

This project is based on this [Hyperledger Developer Tutorial](https://hyperledger.github.io/composer/latest/tutorials/developer-tutorial.html).

### Pre requisites
To run this locally, it is necessary to install Hyperledger Development Tools. All the necessary steps to install and use is [here](https://hyperledger.github.io/composer/latest/installing/development-tools.html).

### Build

After installation process, we first build the business network file.

This is the initial state of the files of the project:

![](https://raw.githubusercontent.com/social-care/blockchain/master/blockchain-tutorial-images/files-beggining.png)

From the application directory, run the following command:
```sh
$ composer archive create -t dir -n .
```
After the command has run, a business network archive file called `blockchain@0.0.1.bna` has been created in the current directory:

![](https://raw.githubusercontent.com/social-care/blockchain/master/blockchain-tutorial-images/files-bna.png)

### Running locally

**All this section explains how to run manually. There is a script that already do this and the build process for you. It's the [start_blockchain.sh](https://github.com/social-care/blockchain/blob/master/start_blockchain.sh) script, leaving only the REST API process to be executed.**

##### Running manually

**You have to get fabric up and runnig before installing this**:
To run Fabric, move to the directory that Fabric has been downloaded and execute the following scripts:
```sh
$ ./startFabric.sh
$ ./createPeerAdminCard.sh
```

*The first time these scripts runs, the download all the images that will be used by the Hyperledger and this step can be very time and bandwidth consuming*

These are the images downloaded from this step:

![](https://raw.githubusercontent.com/social-care/blockchain/master/blockchain-tutorial-images/docker-images.png)

Next, we start the deployment process:

1. To install the business network, from the project directory, run the following command:
```sh
$ composer network install \
    --card PeerAdmin@hlfv1 \
    --archiveFile blockchain@0.0.1.bna
```
2. To start the business network, run the following command:
```sh
$ composer network start \
    --networkName blockchain \
    --networkVersion 0.0.1 \
    --networkAdmin admin \
    --networkAdminEnrollSecret adminpw \
    --card PeerAdmin@hlfv1 \
    --file networkadmin.card
```

This command creates the networkadmin.card file...

![](https://raw.githubusercontent.com/social-care/blockchain/master/blockchain-tutorial-images/file-network-card.png)

... a peer image on docker ...

![](https://raw.githubusercontent.com/social-care/blockchain/master/blockchain-tutorial-images/image-peer.png)

... and a container running that image.

![](https://raw.githubusercontent.com/social-care/blockchain/master/blockchain-tutorial-images/container-peer.png)

3. To import the network administrator identity as a usable business network card, run the following command:
```sh
$ composer card import --file networkadmin.card
```
4. To check that the business network has been deployed successfully, run the following command to ping the network:
```sh
$ composer network ping --card admin@blockchain
```
### REST API

**The REST API is necessary to access the resources of the blockchain**
To create the REST API, navigate to the tutorial-network directory and run the following command:
```sh
$ composer-rest-server
```
Enter `admin@blockchain` as the card name.

Select **never use namespaces** when asked whether to use namespaces in the generated API.

Select **No** when asked wheter to use an APIk key.

Select **No** when asked wheter to enable authentication.

Select **No** when asked whether to secure the generated API.

Select **Yes** when asked whether to enable event publication.

Select **No** when asked whether to enable TLS security.

Now you have blockchain running and a REST API to interface with it on [http://localhost:3000](http://localhost:3000):

![](https://raw.githubusercontent.com/social-care/blockchain/master/blockchain-tutorial-images/rest-api.png)