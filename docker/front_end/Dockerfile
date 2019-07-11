### STAGE 1: Build ###
# We label our stage as ‘builder’
FROM node:10-alpine as builder
COPY package.json package-lock.json ./
## Storing node modules on a separate layer will prevent unnecessary npm installs at each build
RUN npm ci && mkdir ng-app && npm install bootstrap --save && npm install jquery --save && npm install cors --save && npm install body-parser --save && npm install mongoose --save && mv ./node_modules ./ng-app
WORKDIR ./ng-app
COPY . .
## Build the angular app in production mode and store the artifacts in dist folder
RUN npm run ng build -- --prod --output-path=dist
CMD [ "node", "src/server.js" ]
### STAGE 2: Setup ###
FROM nginx:1.14.1-alpine
## Copy our default nginx config
COPY nginx/default.conf /etc/nginx/conf.d/
## Remove default nginx website
RUN rm -rf /usr/share/nginx/html/*
## From ‘builder’ stage copy over the artifacts in dist folder to default nginx public folder
COPY --from=builder /ng-app/dist /usr/share/nginx/html
CMD ["nginx", "-g", "daemon off;"]
### STAGE 3: Setup ###
#FROM node:10-alpine
#RUN mkdir /application
#COPY --from=builder /ng-app /application
#COPY src /application
#WORKDIR /application
#CMD [ "node", "src/server.js" ]
