FROM docker.io/library/node:21-alpine3.18 AS BUILDER

WORKDIR /app

COPY package.json ./
COPY package-lock.json ./
COPY src/ ./
COPY tsconfig.json ./
COPY .env.sample ./.env

RUN npm ci

COPY . .

RUN npm run build

FROM docker.io/library/node:21-alpine3.18 AS RUNNER

WORKDIR /app

COPY package.json ./
COPY package-lock.json ./
COPY --from=BUILDER /app/dist ./dist
COPY --from=BUILDER /app/.env ./

EXPOSE 8888

COPY --from=BUILDER /app/node_modules ./node_modules

CMD [ "npm", "start" ]
