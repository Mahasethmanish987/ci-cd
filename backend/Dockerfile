#----------- Build Stage -------------
FROM node:20-alpine3.22 AS build

RUN apk update && apk upgrade --no-cache

WORKDIR /app

COPY package.json package-lock.json ./

RUN npm ci --omit=dev && npm cache clean --force

#----------- Production Stage -------------
FROM node:20-alpine3.22 AS production

RUN apk update && apk upgrade --no-cache

RUN addgroup -S appgroup && \
    adduser -S appuser -G appgroup

RUN apk add --no-cache dumb-init=1.2.5-r3

WORKDIR /app

COPY --from=build /app/node_modules ./node_modules

COPY src ./src
COPY package.json ./

USER appuser

EXPOSE 5000

ENTRYPOINT ["dumb-init", "--"]

CMD ["node", "src/index.js"]