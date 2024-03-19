FROM node:18-alpine

WORKDIR /usr/src/backend

COPY package*.json ./

RUN npm install

EXPOSE 3000

COPY . .

CMD [ "node", "index.js" ]
