# This is an intentionally vulnerable Dockerfile 
# Using Node.js 14.17.0 which has multiple CVEs

FROM node:14.17.0

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
COPY package*.json ./
RUN npm install --omit=dev

# Bundle app source
COPY . .

EXPOSE 3000
CMD [ "node", "server.js" ] 