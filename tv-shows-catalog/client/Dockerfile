FROM node:18

WORKDIR /client

COPY . .

COPY .env.example .env

RUN npm install

EXPOSE 5173

CMD ["npm", "run", "dev"]