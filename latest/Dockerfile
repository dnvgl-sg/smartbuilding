ARG NODE_VERSION=6
FROM node:${NODE_VERSION}

# Home directory for Node-RED application source code.
RUN mkdir -p /usr/src/node-red

# User data directory, contains flows, config and nodes.
RUN mkdir /data

# Timezone settings
RUN echo "Asia/Singapore" > /etc/timezone

WORKDIR /usr/src/node-red

# Add node-red user so we aren't running as root.
RUN useradd --home-dir /usr/src/node-red --no-create-home node-red \
    && chown -R node-red:node-red /data \
    && chown -R node-red:node-red /usr/src/node-red
    
USER node-red

# package.json contains Node-RED NPM module and node dependencies
COPY package.json /usr/src/node-red/
COPY settings.js /data/

COPY flows.json /data/
COPY rpi_meeting.jpg /data/

RUN npm install
RUN npm install node-red-dashboard node-red-contrib-web-worldmap node-red-contrib-azure-iot-hub node-red-admin node-red-node-mysql node-red-admin node-red-contrib-python3-function node-red-contrib-spreadsheet-in node-red-node-darksky

# User configuration directory volume
EXPOSE 1880

# Environment variable holding file path for flows configuration
ENV FLOWS=flows.json
ENV NODE_PATH=/usr/src/node-red/node_modules:/data/node_modules

CMD ["npm", "start", "--", "--userDir", "/data"]
