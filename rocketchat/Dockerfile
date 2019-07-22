FROM node:alpine
LABEL maintainer="Simon Suprun <simonsuprun@gmail.com>"

ENV RC_VERSION 1.1.3

WORKDIR /home/

# Create accounts
RUN addgroup -g 1100 rocketchat \
 && adduser -Ss /bin/false -u 1100 -G rocketchat -h /app rocketchat \
 && mkdir -p /app/uploads \
 && chown rocketchat:rocketchat /app/uploads

# Prepare environment
RUN apk add --update \
    curl \
    tar \
    python \
    make \
    g++ \
 && curl -ssL https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub -o /etc/apk/keys/sgerrand.rsa.pub \
 && curl -ssL https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.28-r0/glibc-2.28-r0.apk -o glibc-2.28-r0.apk \
 && apk add glibc-2.28-r0.apk \
 && rm -rf glibc-2.28-r0.apk \
 && curl -O https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh \
 && chmod +x wait-for-it.sh \
 && mv wait-for-it.sh /usr/local/bin

# Download and install Rocket.Chat
RUN curl -fSL "https://releases.rocket.chat/${RC_VERSION}/download" -o rocket.chat.tgz \
 && tar zxf rocket.chat.tgz \
 && rm rocket.chat.tgz \
 && cd bundle/programs/server \
 && npm install \
 && npm cache clear --force

# Clear environment
RUN apk del curl tar python make g++

USER rocketchat

WORKDIR /home/rocketchat/bundle/

COPY docker-entrypoint.sh /usr/local/bin/

ENTRYPOINT ["docker-entrypoint.sh"]
