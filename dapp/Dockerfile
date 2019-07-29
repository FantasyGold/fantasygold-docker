FROM ubuntu as tools
MAINTAINER nicofgc 

RUN apt-get update && \
  apt-get install -y wget unzip && \
  apt-get install -y curl vim-tiny less && \
  rm -rf /var/lib/apt/lists/*

RUN wget https://github.com/mattn/goreman/releases/download/v0.0.10/goreman_linux_amd64.zip && \
  unzip -o -d /usr/local/bin goreman_linux_amd64.zip && \
  rm goreman_linux_amd64.zip

ENV FGC_RELEASE 2.0.0
ENV FGC_RELEASE_TAR fantasygold-${FGC_RELEASE}-Linux64.tar.gz

RUN wget https://github.com/fantasygold/fantasygold-core/releases/download/-v${FGC_RELEASE}/${FGC_RELEASE_TAR} && \
  tar -xf $FGC_RELEASE_TAR -C /usr/local --strip-components=1 --exclude=*-qt --exclude=test_fantasygold --exclude=fantasygold-tx && \
  rm $FGC_RELEASE_TAR

RUN wget https://github.com/ethereum/solidity/releases/download/v0.4.24/solc-static-linux -O /usr/local/bin/solc && \
   chmod 0755 /usr/local/bin/solc

RUN wget -v https://github.com/fantasygold/solar/releases/download/0.0.14/solar-linux-amd64 -O /usr/local/bin/solar && chmod 0755 /usr/local/bin/solar 

RUN wget https://github.com/fantasygold/fantasygoldportal/releases/download/0.0.8/fantasygoldportal-linux -O /usr/local/bin/fantasygoldportal && chmod 0755 /usr/local/bin/fantasygoldportal

# Use multi-stage build makes the image smaller by about 80 MB
FROM jeanblanchard/alpine-glibc

# RUN apt-get update && \
#   apt-get install -y curl vim-tiny less && \
#   rm -rf /var/lib/apt/lists/*

ENV FGC_DATADIR /dapp/.fantasygold
ENV FGC_RPC_USER fantasygold
ENV FGC_RPC_PASS test
ENV FGC_RPC http://$FGC_RPC_USER:$FGC_RPC_PASS@localhost:57810
ENV FGC_NETWORK regtest

VOLUME /dapp

COPY --from=tools /usr/local/bin /usr/local/bin

COPY fcli /usr/local/bin
COPY fantasygoldd-launch /usr/local/bin
COPY Procfile /Procfile

# 9888: fantasygoldportal DApp RPC service
# 9899: fantasygoldportal auth service
# 57810: fantasygoldd RPC
# 57814: peering port
# 58806: peering port (testnet)
EXPOSE 9888 9899 57810 57814 58806

WORKDIR /dapp

# 172.17.0.0/16 is the network submask for docker's bridge network.
# -rpcallowip=172.17.0.0/16 permits RPC access from docker host
CMD mkdir -p $FGC_DATADIR && goreman -f /Procfile start
