FROM node:18-alpine

WORKDIR /client

COPY . .

RUN npm install

# arguments de build 
ARG CLIENT_PROD_PORT
ARG SERVER_PORT

# écrire le base url dans un .env
RUN echo "API_BASE_URL=http://tv-shows-server-prod:$SERVER_PORT" > .env

EXPOSE $CLIENT_PROD_PORT

RUN npm run build

CMD ["node", "build/index.js"]


