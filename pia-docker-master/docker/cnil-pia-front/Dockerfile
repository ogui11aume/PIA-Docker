FROM node:22.20 as builder
# FROM node:18.18.2-alpine3.18@sha256:1c19c701b3f22fa88681630d3acac968b1b0b944b51b57e17041d37ddccab579 as builder

USER root

ARG PIA_VERSION=v4.1.0
ARG PIA_BRANCH=sso

ARG ANGULAR_CLI_VERSION=20.3

RUN npm install -g @angular/cli@$ANGULAR_CLI_VERSION

RUN wget https://github.com/LINCnil/pia/archive/$PIA_BRANCH.zip \
 && unzip $PIA_BRANCH.zip \
 && rm $PIA_BRANCH.zip
RUN mkdir pia-front
COPY pia-$PIA_BRANCH pia-front
WORKDIR pia-front

RUN corepack enable
RUN corepack prepare yarn@4.10.3 --activate

RUN yarn install
RUN yarn prod
RUN mkdir -p /dist
RUN mv dist/pia-angular /dist/pia

FROM nginx:1.29 as runtime

RUN mkdir -p /var/www && chown www-data -R /var/www/

COPY ./conf/cnil_pia.conf /etc/nginx/conf.d/default.conf

COPY --from=builder /dist/pia/browser /var/www/
