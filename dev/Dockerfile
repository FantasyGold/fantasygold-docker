FROM ubuntu
MAINTAINER nicofgc 

RUN set -ex \
    && apt-get update \
    && apt-get install -y -qq --no-install-recommends ca-certificates git wget apt-utils \
    && apt-get install -y build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils \
    && apt-get install -y libboost-all-dev \
	&& apt-get install -y software-properties-common \
	&& add-apt-repository -y ppa:bitcoin/bitcoin \
	&& apt-get update \
	&& apt-get install -y libdb4.8-dev libdb4.8++-dev \
	&& apt-get install -y libzmq3-dev \
 	&& rm -rf /var/lib/apt/lists/*

# install fantasygold from github
RUN cd /tmp && git clone --recursive https://github.com/FantasyGold/FantasyGold-Core.git \
	&& cd fantasygold \
	&& ./autogen.sh \
	&& ./configure --without-gui \
	&& make \
	&& make check \
	&& make install  \
	&& rm -rf /tmp/*

# create data directory
ENV FGC_DATA /data
RUN mkdir $FGC_DATA \
	&& ln -sfn $FGC_DATA /root/.fantasygold \
VOLUME /data

EXPOSE 57814 57810 58806 59806
CMD ["fantasygoldd"]
