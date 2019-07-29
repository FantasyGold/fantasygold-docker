# Quickstart

This is a fantasygold-qt image, launch GUI wallet

## Get docker image

To get the latest image, you might take either way:

### Pull a image from Public Docker hub

```
$ docker pull fantasygold/fantasygold-qt
```

### Or, build fantasygold image with provided Dockerfile

```
$docker build --rm -t fantasygold/fantasygold-qt .
```

For historical versions, please visit [docker hub](https://hub.docker.com/r/fantasygold/fantasygold-qt/)

## Prepare data path & fantasygold.conf

In order to use user-defined config file, as well as save block chain data, -v option for docker is recommended.

First chose a path to save fantasygold block chain data:

```
sudo rm -rf /data/fantasygold-data
sudo mkdir -p /data/fantasygold-data
sudo chmod a+w /data/fantasygold-data
```

Create your config file. Then please create the file ${PWD}/fantasygold.conf with content:

```
rpcuser=fantasygold
rpcpassword=fantasygoldtest
```

User can set their own config file on demands.

## Launch fantasygold-qt

For Linux:

```
$ docker run -it --rm \
             -v /tmp/.X11-unix:/tmp/.X11-unix \
             -v ${PWD}/fantasygold.conf:/root/.fantasygold/fantasygold.conf \
             -v /data/fantasygold-data/:/root/.fantasygold/ \
             -e DISPLAY  fantasygold/fantasygold-qt
```

For Mac:

Please refer to
[https://cntnr.io/running-guis-with-docker-on-mac-os-x-a14df6a76efc](https://cntnr.io/running-guis-with-docker-on-mac-os-x-a14df6a76efc) about how to run gui with docker on mac.

```
## install & launch socat
$ brew install socat
$ socat TCP-LISTEN:6000,reuseaddr,fork UNIX-CLIENT:\"$DISPLAY\"

## install & open Xquartz
$ brew install xquartz
$ open -a Xquartz

## then set Xquartz preferences "Security-'Allow connections from network clients'"

## launch fantasygold-qt 
$ docker run -e DISPLAY=<your_ip>:0 -v ${PWD}/fantasygold.conf:/root/.fantasygold/fantasygold.conf -v /data/fantasygold-data/:/root/.fantasygold/ fantasygold/fantasygold-qt

```


`${PWD}/fantasygold.conf` will be used, and blockchain data saved under /data/fantasygold-data/


## exit fantasygold-qt

Exit the gui wallet in normal way.


