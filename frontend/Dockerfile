FROM node:12.16.3-slim

# RUN yarn global add @vue/cli

WORKDIR /usr/src/frontend/

COPY ./frontend/package.json ./frontend/yarn.lock ./

RUN yarn install

COPY ./frontend /usr/src/frontend
