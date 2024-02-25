FROM node:16-alpine

WORKDIR /server

COPY . .

RUN npm install

EXPOSE 3000

RUN npm run build

CMD ["npm", "run", "start:prod"]



