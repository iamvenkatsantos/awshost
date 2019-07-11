FROM node:10-alpine
COPY . admission
WORKDIR admission
# Storing node modules on a separate layer will prevent unnecessary npm installs at each build
RUN npm ci && npm install cors --save && npm install body-parser --save && npm install mongoose --save  
RUN mkdir build && mv node_modules build && mv src build 
RUN cd /admission/build
WORKDIR build
CMD [ "node", "src/server.js" ]
