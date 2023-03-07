FROM node:16.18

ENV PORT=3000

WORKDIR /frontend-react-js

COPY . ./

RUN npm install

COPY . .

EXPOSE ${PORT}

CMD ["npm", "start"]