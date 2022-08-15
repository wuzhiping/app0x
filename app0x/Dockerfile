FROM node:lts
WORKDIR /opt/bpm

RUN npm install pm2 -g
ADD package.json /opt/bpm
RUN npm install --loglevel verbose

ADD start.sh /opt/bpm
ADD bpm.js /opt/bpm/bpm.js
Add openApi.js /opt/bpm/openApi.js
