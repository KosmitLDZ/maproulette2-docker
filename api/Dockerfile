FROM adoptopenjdk/openjdk8:jdk8u212-b03

## Add the User
RUN adduser -system --gid 0 maproulette

## Apt-Get for basic packages
RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y apt-transport-https gnupg2 \
    && echo "deb https://dl.bintray.com/sbt/debian /" | tee -a /etc/apt/sources.list.d/sbt.list \
    && apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 642AC823 \
    && apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y scala \
        sbt \
        unzip \
        wget \
        git \
        openssh-server \
        jq \
        g++ \
        build-essential

ARG gitRepo=https://github.com/maproulette/maproulette2.git
ARG gitTag=LATEST

# Download Maproulette V2
RUN git clone ${gitRepo} /maproulette-api \
    && chmod 777 /maproulette-api \
    && cd maproulette-api \
    && if [ "${gitTag}" != "LATEST" ]; then git checkout tags/${gitTag} ; fi

# package Maproulette V2
WORKDIR /maproulette-api
ARG apiHost=maproulette.org
RUN export API_HOST=${apihost} \
    && sbt clean compile dist \
    && unzip -d / target/universal/MapRouletteAPI.zip

ADD docker.conf	/MapRouletteAPI/conf/docker.conf
ADD osmcacerts /MapRouletteAPI/conf/

CMD /MapRouletteAPI/bin/maprouletteapi -Dhttp.port=$PORT -Dconfig.resource=docker.conf \
    -Ddb.default.url=${DB_URL} \
    -Ddb.default.username=${DB_USER} \
    -Ddb.default.password=${DB_PASSWORD} \
    -Dosm.consumerKey=${CONSUMER_KEY} \
    -Dosm.consumerSecret=${CONSUMER_SECRET} \
    -Dsuper.key=${SUPER_KEY} \
    -Dsuper.accounts=${SUPER_ACCOUNTS} \
    -Djavax.net.ssl.trustStore=/MapRouletteAPI/conf/osmcacerts \
    -Djavax.net.ssl.trustStorePassword=openstreetmap
