FROM node:19-alpine3.16

RUN mkdir /node

WORKDIR /node

#COPY package.json ./
#
#RUN npm install -g npm@8.19.3

RUN npm init -y

#RUN npm install -g @vue/cli
#CMD "npm init vue@latest -y"

COPY . .

EXPOSE 5173
