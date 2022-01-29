# syntax=docker/dockerfile:1

FROM node:16.13.2-alpine AS build

ENV NODE_ENV=production

WORKDIR /app

COPY ["package.json", "package-lock.json*", "./"]

RUN npm install --production

COPY . .

RUN npm run build

FROM nginx AS final

WORKDIR /usr/share/nginx/html

COPY --from=build /app/build .