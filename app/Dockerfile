FROM node:current-alpine3.15

EXPOSE 3000

COPY ./bootcamp-node-project-*.tgz /var/app/

RUN tar -xzvf /var/app/bootcamp-node-project-*.tgz -C /var/app

WORKDIR /var/app/package

RUN npm install

ENTRYPOINT ["npm", "start"]

