# Quickstart

## Get docker image

You might take either way:

### Pull a image from Public Docker hub

```
$ docker pull fantasygold/fantasygold
```

### Or, build fantasygold image with provided Dockerfile

```
$docker build --rm -t fantasygold/fantasygold .
```

For historical versions, please visit [docker hub](https://hub.docker.com/r/fantasygold/fantasygold/)

## Prepare data path and fantasygold.conf

In order to use user-defined config file, as well as save block chain data, -v option for docker is recommended.

First chose a path to save fantasygold block chain data:

```
sudo rm -rf /data/fantasygold-data
sudo mkdir -p /data/fantasygold-data
sudo chmod a+w /data/fantasygold-data
```

Create your config file. Note rpcuser and rpcpassword to required for later `fantasygold-cli` usage for docker, so it is better to set those two options. Then please create the file ${PWD}/fantasygold.conf with content:

```
rpcuser=fantasygold
rpcpassword=fantasygoldtest
```
## Launch fantasygoldd

To launch fantasygold node:

```
## to launch fantasygoldd
$ docker run -d --rm --name fantasygold_node \
             -v ${PWD}/fantasygold.conf:/root/.fantasygold/fantasygold.conf \
             -v /data/fantasygold-data/:/root/.fantasygold/ \
             fantasygold/fantasygold fantasygoldd

## check docker processed
$ docker ps

## to stop fantasygoldd
$ docker run -i --network container:fantasygold_node \
             -v ${PWD}/fantasygold.conf:/root/.fantasygold/fantasygold.conf \
             -v /data/fantasygold-data/:/root/.fantasygold/ \
             fantasygold/fantasygold fantasygold-cli stop
```

`${PWD}/fantasygold.conf` will be used, and blockchain data saved under /data/fantasygold-data/

## Interact with `fantasygoldd` using `fantasygold-cli`

Use following docker command to interact with your fantasygold node with `fantasygold-cli`:

```
$ docker run -i --network container:fantasygold_node \
             -v ${PWD}/fantasygold.conf:/root/.fantasygold/fantasygold.conf \
             -v /data/fantasygold-data/:/root/.fantasygold/ \
             fantasygold/fantasygold fantasygold-cli getinfo
```

For more fantasygold-cli commands, use:

```
$ docker run -i --network container:fantasygold_node \
             -v ${PWD}/fantasygold.conf:/root/.fantasygold/fantasygold.conf \
             -v /data/fantasygold-data/:/root/.fantasygold/ \
             fantasygold/fantasygold fantasygold-cli help
```

