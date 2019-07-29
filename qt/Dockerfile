FROM ubuntu
MAINTAINER nicofgc

RUN set -ex \
    && apt-get update \
    && apt-get install -y -qq --no-install-recommends ca-certificates curl wget apt-utils jq \
    && apt-get install -y libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools protobuf-compiler qrencode

# install fantasygold binaries
RUN set -ex \
    && echo `curl -s https://api.github.com/repos/fantasygold/fantasygold-core/releases/latest | jq -r ".assets[] | select(.name | test(\"Linux64.tar.gz\")) | .browser_download_url"` > /tmp/fantasygold_url \
    && FGC_URL=`cat /tmp/fantasygold_url` \
    && FGC_DIST=$(basename $FGC_URL) \
    && wget -O $FGC_DIST $FGC_URL \
	&& tar -xzvf $FGC_DIST -C /usr/local --strip-components=1 \
	&& rm /tmp/fantasygold* \
	&& rm -rf /var/lib/apt/lists/*

# create data directory
ENV FGC_DATA /data
RUN mkdir $FGC_DATA \
	&& ln -sfn $FGC_DATA /root/.fantasygold \
VOLUME /data

EXPOSE 57814 57810 58806 59806
CMD ["fantasygold-qt"]
