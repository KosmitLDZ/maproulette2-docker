FROM node:10

RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y jq git

ARG gitRepo=https://github.com/maproulette/maproulette2.git
ARG gitTag=LATEST

RUN git clone ${gitRepo} /maproulette-frontend \
    && chmod 777 /maproulette-frontend \
    && cd maproulette-frontend \
    && if [ "${gitTag}" != "LATEST" ]; then git checkout tags/${gitTag} ; fi

ENV NODE_OPTIONS="--max-old-space-size=8192"
WORKDIR /maproulette-frontend
RUN yarn install \
    && yarn run build
RUN yarn global add serve

CMD serve -s build -l 3000
