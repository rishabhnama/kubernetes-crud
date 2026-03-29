FROM node:17-alpine

WORKDIR /var/dbapp

# COPY ./index.js ./

COPY ./package.json ./

RUN npm install

COPY . .

CMD ["node", "index.js"]
